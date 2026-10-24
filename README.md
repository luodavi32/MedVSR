# MedVSR

Mandarin medical lip-reading dataset: 305,672 sentence-level clips, 56 speakers, 288 hours, with overlap and unseen-speaker splits.

(The complete dataset will be updated after the paper is accepted…)

MedVSR is a benchmark for sentence-level medical visual speech recognition. Clips come from public Douyin and Bilibili medical videos. Each sample is a spoken sentence with an ASR-derived character transcript. The official metric is sentence-level character error rate (CER) against that transcript. Inference is visual-only.

| | |
|---|---|
| Language | Mandarin |
| Speakers | 56 |
| Utterances | 305,672 |
| Duration | 288 hours |
| Vocabulary | 3,185 symbols (Chinese characters, letters, and symbols) |
| Transcript length | 5–45 characters (mean 16.7) |
| Video | 25 fps, 88×88 mouth crop |
| Label | ASR-derived character sequence |

Overlap and unseen are two partitions of the same clip pool. Do not add their counts together.

| Protocol | Split | Speakers | Utterances | Hours |
|---|---|---:|---:|---:|
| Overlap | Train | 56 | 244,461 | 230 |
| Overlap | Val | 56 | 30,655 | 29 |
| Overlap | Test | 56 | 30,556 | 29 |
| Unseen | Train | 40 | 224,400 | 210 |
| Unseen | Val | 8 | 32,302 | 33 |
| Unseen | Test | 8 | 48,970 | 45 |

In the overlap protocol, a speaker may appear in more than one split. In the unseen protocol, the 40 training speakers, 8 validation speakers, and 8 test speakers are disjoint. Unseen validation and test counts include both the adaptation subset and the recognition subset. Official unseen CER uses the recognition subset only.

Unseen test speakers: S002, S009, S018, S028, S029, S032, S035, S047.

Clips from the same source video stay in one split.

## What is in this repository

| Path | Content | Status |
|---|---|---|
| `labels/` | CSV manifests | Included |
| `dictionary/zh-cn/medvsr.txt` | Decoder dictionary | Included |
| `video/` | Mouth-region clips | Empty. Uploaded after the paper is accepted |
| `audio/` | Clip audio | Empty. Uploaded after the paper is accepted |

```
labels/
  train.csv
  val.csv
  test.csv
  unseen_train.csv
  unseen_val.csv
  unseen_test.csv
  adapt/
dictionary/
  zh-cn/
    medvsr.txt
video/
audio/
```

`video/` and `audio/` are placeholders. Mouth-region video and the corresponding audio will be uploaded after the paper is accepted. The CSV manifests and the dictionary are enough to inspect the transcripts, splits, and output alphabet. They are not enough to train or evaluate a model.

## CSV manifests

Files in `labels/` are UTF-8 CSV with a header. One row is one clip.

| Column | Meaning |
|---|---|
| `dataset_name` | Always `MedVSR` |
| `relative_path` | Clip path under `video/`. Speaker ID is the `Sxxx` directory. The next directory is the source video. |
| `frame_length` | Number of frames. At 25 fps, duration in seconds is `frame_length / 25` |
| `label` | Official transcript. This string is the CER reference |

Example:

```text
dataset_name,relative_path,frame_length,label
MedVSR,MedVSR_video_seg24s/S001/<source_video>/S001_<source_video>_clip0001_004285_004463.mp4,178,<transcript>
```

A clip is resolved as `video/<relative_path>` once the video files are released. The stem of that path (without `.mp4`) is the clip ID used for the matching audio file.

| File | Protocol | Split |
|---|---|---|
| `labels/train.csv` | Overlap | Train |
| `labels/val.csv` | Overlap | Val |
| `labels/test.csv` | Overlap | Test |
| `labels/unseen_train.csv` | Unseen | Train, 40 speakers |
| `labels/unseen_val.csv` | Unseen | Val, 8 speakers, adaptation and recognition |
| `labels/unseen_test.csv` | Unseen | Test, 8 speakers, adaptation and recognition |

`labels/adapt/` holds the fixed per-speaker adaptation lists at 1-, 3-, and 5-minute budgets. These lists are drawn from a speaker's adaptation subset. Recognition clips are not included. Use them only for unseen-speaker adaptation. Report official unseen CER on the recognition subset.


## Dictionary

`dictionary/medvsr.txt` is the closed decoder alphabet: 3,185 symbols, one token per line, UTF-8. The alphabet contains Chinese characters, letters, and symbols used in medical expressions. It is not a list of sentence classes, and it is not a medical-term tag set.

The six medical-term types used in the paper (disease, drug, examination or treatment, anatomy or physiology, symptom or sign, dose or unit) are dictionary matches on the transcripts. They describe the corpus. They are not prediction labels.

## Data(mp4/wav)


…


## License

MedVSR is distributed for non-commercial research. Mouth crops are biometric data. The license does not allow diagnosis, treatment recommendation, surveillance, re-identification, or identity recognition.

## Citation

…
