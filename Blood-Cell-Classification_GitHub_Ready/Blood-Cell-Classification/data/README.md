# Local dataset layout

The default workflow downloads `unclesamulus/blood-cells-image-dataset` with KaggleHub. To use existing files, set `BLOOD_CELL_DATA_DIR` to the directory containing the class folders, or its parent if it contains a `bloodcells_dataset` directory.

Example paths:

- `bloodcells_dataset/basophil/example.jpg`
- `bloodcells_dataset/eosinophil/example.jpg`
- `bloodcells_dataset/erythroblast/example.jpg`
- `bloodcells_dataset/lymphocyte/example.jpg`
- `bloodcells_dataset/monocyte/example.jpg`
- `bloodcells_dataset/platelet/example.jpg`

Keep readable images directly inside each class folder. Supported extensions are `.jpg`, `.jpeg`, `.png`, `.bmp`, `.tif`, and `.tiff`, case-insensitively. Nested image directories are not traversed. Extension filtering does not guarantee that an image is readable; corrupted images must be repaired or removed.

Folders named `ig` and `neutrophil` are skipped. Other image-containing subdirectories are treated as classes, so keep unrelated folders outside the selected dataset directory. Provide all six intended classes.

For local storage inside the repository, `data/bloodcells_dataset/` can be used; it is ignored by Git. Set the environment variable accordingly. Do not upload images or credentials with the project. Verify the dataset source's license and citation requirements separately.
