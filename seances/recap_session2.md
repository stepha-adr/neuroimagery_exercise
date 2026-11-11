# Recap – Session 2 (11 November 2026)

## What we did

1. **Homework 1 review**: images as arrays, histogram, threshold, mask, and why a threshold cannot separate the brain from the skull (question 6).
2. **A short introduction to MRI** (slides "MRI in 15 minutes" and "Spin echo vs gradient echo").
3. **From MRI to fMRI**: the BOLD signal, and fMRI data as a 4D array.
4. **Three Nipraxis notebooks together**:
   - [Working with four dimensional images, masks and functions](https://textbook.nipraxis.org/intro_to_4d.html)
   - [Reshaping, 4D to 2D](https://textbook.nipraxis.org/reshape_and_4d.html)
   - [Voxels and time](https://textbook.nipraxis.org/voxels_by_time.html)

---

## 1. MRI in a nutshell

| Idea | What to remember |
|---|---|
| **Signal** | Hydrogen protons line up in the scanner's field (B0). A radio pulse tips them over; as they relax they send back a radio echo. A Fourier transform turns it into one number per voxel. |
| **T1** | Time for the protons to realign with the field. Fat: fast. CSF: slow. |
| **T2** | Time for the echo to fade because protons fall out of phase. CSF keeps its echo longest. |
| **TR / TE** | Repetition time and echo time. Short TR and TE give **T1-weighted** images; long TR and TE give **T2-weighted** images. |
| **T1-weighted image** | Fat bright, white matter light, grey matter grey, CSF dark. This is why your homework 1 mask kept the scalp and had holes at the ventricles. |
| **T2\*** | The observed decay, including static field differences inside the voxel: 1/T2\* = 1/T2 + 1/T2′. Always shorter than T2. |
| **Spin echo vs gradient echo** | A spin echo uses a 180° pulse that cancels static field differences, so it measures T2. A gradient echo does not, so it is **T2\*-weighted** and sensitive to anything that distorts the local field. |
| **BOLD** | Deoxyhaemoglobin is paramagnetic and distorts the field around vessels. During activity, blood flow brings more oxygenated blood than is used, so deoxyhaemoglobin drops, T2\* gets longer and the signal rises by a few %. The response is slow: it peaks about 5 s after the stimulus. |
| **Why 7 T** | BOLD contrast and signal-to-noise grow with field strength, so pRFs can be measured more precisely. |

---

## 2. fMRI data are 4D

The example run `ds114_sub009_t2r1.nii` (Nipraxis textbook data):

| | Value | In code |
|---|---|---|
| Voxels per volume | 64 × 64 × 30 | `data.shape[:3]` |
| Voxel size | 4 mm | `img.header.get_zooms()[:3]` |
| TR (one volume every…) | 2.5 s | `img.header.get_zooms()[3]` |
| Number of volumes | 173 | `data.shape[3]` |

**What we saw in the data:** the first volume is much brighter than the others, about 414 against a mean of 337. At that point the magnetisation has not reached its steady state yet, which is why the first volumes are usually discarded.

---

## 3. Code to remember

```python
img = nib.load(nipraxis.fetch_file('ds114_sub009_t2r1.nii'))
data = img.get_fdata()                 # shape (64, 64, 30, 173)

vol = data[..., 0]                     # one volume, shape (64, 64, 30)
tc = data[32, 32, 15, :]               # one voxel's time course, shape (173,)

data_2d = data.reshape(-1, 173)        # voxels × time, shape (122880, 173)
vol_means = data_2d.mean(axis=0)       # one value per volume (average over voxels)
voxel_means = data_2d.mean(axis=1)     # one value per voxel (average over time)
back_3d = voxel_means.reshape(64, 64, 30)   # back to an image

mask = voxel_means > voxel_means.mean()     # rough brain mask (1D, one value per voxel)
in_brain = data_2d[mask]                     # shape (n voxels in mask, 173)
```

**Rules of thumb:**

- `reshape` never changes the values, only how they are arranged. Reshaping back with the original shape gives the original array.
- The **last** axis changes fastest. That's why `reshape(-1, n_vols)` keeps each voxel's whole time course on one row.
- `axis=0` averages down the rows (over voxels). `axis=1` averages along the columns (over time).
- Before running a line, predict the shape of the result.

---

## 4. Homework 2, due before session 3

The notebook `homework2.ipynb` goes in your `neuroimagery_exercise` repository.

- **Part 0:** reminders on shape, reshape order and for loops.
- **Part 1:** a small toy array, to see exactly where each number goes when you reshape.
- **Part 2:** the dimensions of the real fMRI run: number of voxels, volumes, run duration, and what each kind of indexing returns.
- **Part 3:** volume means, first with a for loop and then with reshape, plus a check that the reshape kept the voxels in order.
- **Part 4:** a temporal signal-to-noise (tSNR) map, going from 2D back to 3D, and the effect of the first volume.
- **Part 5:** a loop over slices: where is tSNR lowest, and why (the link to T2\*).
- **Bonus:** turn your code into two small functions and test them with `assert`.

Don't forget: run `git pull` before you start, then `add`, `commit`, `push` when you're done.

---

**Next session (session 3), 25 November:** convolution, the haemodynamic response function (HRF) and the general linear model (GLM).
