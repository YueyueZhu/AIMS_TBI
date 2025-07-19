# AIMS\_TBI Prediction Docker Guide

This guide will walk you through how to load and run the Docker image for this project, which is used for brain image segmentation.

---

## 📦 1. Load the Docker Image

First, make sure you have the following items in the same directory as the `Dockerfile`:

* `aims_tbi_pred.tar.gz`  — the docker image, which has my Model and Model Weight
* In this image, my code path is: /workspace/AIMS_TBI
* My weight path is: /workspace/AIMS_TBI/parameter/best_model_AIMS_13.pth

**To load the image, run:**

```bash
gunzip -c aims_tbi_pred.tar.gz > aims_tbi_pred.tar

docker load -i aims_tbi_pred.tar
```

---

## 📁 2. Prepare Your Data

The test data should consist of `.nii.gz` format images, for example:

```
scan_0001_T1.nii.gz
scan_0017_T1.nii.gz
scan_0036_T1.nii.gz
...
```

Place these files in a folder like:

```
/home/user/test_data/
```

---

## 🚀 3. Run the Container with Mounted Data

Use the `-v` option to mount your test data and specify the prediction output directory.

Because this image contains code and weights, only the test data input and prediction output paths need to be mapped.

```bash
docker run -it --gpus all --name aims_tbi_pred_container \
  -v /home/user/test_data:/data/test_data \
  -v /home/user/test_pred_mask:/data/test_pred_mask \
  aims_tbi_pred
```

Explanation:

| Option                 | Description                          |
| ---------------------- | ------------------------------------ |
| `/data/test_data`      | Maps to your test data folder        |
| `/data/test_pred_mask` | Prediction output will be saved here |

---

## 🧪 4. Run the Prediction Script

Once inside the container, run the following command:

```bash
python predict.py \
  --image_dir /data/test_data \
  --image_process_dir /data/tmp_process \
  --pred_save_path /data/test_pred_mask
```

**Parameter Descriptions:**

| Parameter             | Description                                         |
| --------------------- | --------------------------------------------------- |
| `--image_dir`         | Path to raw `.nii.gz` images (mounted path)         |
| `--image_process_dir` | Temporary directory for image preprocessing         |
| `--pred_save_path`    | Directory to save prediction results (mounted path) |

---

## ✅ Output

The prediction results will be saved in your mounted `/data/test_pred_mask` directory, for example:

```
/home/user/test_pred_mask/scan_0001_T1_pred.nii.gz
/home/user/test_pred_mask/scan_0017_T1_pred.nii.gz
...
```

---

## 💬 Contact

If you have any questions, please contact the project maintainer or submit an issue (1318655459@qq.com || 15855951818@163.com).
