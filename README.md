# Word-Level Audio Perception MVP on OCI Data Science

## Introduction

Turning an audio-research hypothesis into a testable experience should not require building a full product first.
This MVP combines Gradio, Whisper, Librosa and SciPy in an OCI Data Science Notebook Session.
An administrator records or uploads a sentence, transcribes it, reviews word-level timing and adjusts audio parameters.
The application transforms each word, concatenates the resulting sequence and presents it to an end user for a listening test.
OCI Data Science supplies the managed JupyterLab workspace, selectable Compute, persistent storage and an easy start/stop lifecycle.
This lets teams focus on the experimental flow rather than notebook-server administration during early iterations.
The scope is deliberately limited to a demonstrator: it is not a validated scientific protocol or production application.
It is designed to be copied, inspected and adapted for further audio, perception or user-research experiments.
Gradio's sharing link is useful for a non-sensitive demo; a production architecture requires separate hosting and security controls.

Companion repository for the Medium tutorial **Build an Interactive Audio Perception MVP with Gradio and OCI Data Science**.

The Medium-ready article source is available in [`articles/medium-article.md`](articles/medium-article.md).

## Contents

- `notebooks/word-level-audio-perception-gradio-oci.ipynb` — complete tutorial notebook
- `articles/medium-article.md` — source version of the companion Medium tutorial
- `requirements.txt` — Python package versions validated in the tested OCI Data Science environment
- `assets/` — screenshots used by the tutorial

## Run in OCI Data Science

1. Create and activate an OCI Data Science Notebook Session with outbound internet access.
2. Upload the notebook to JupyterLab.
3. Run the cells in order, including package and FFmpeg installation.
4. Open the Gradio link emitted by `demo.launch(share=True)` only for non-sensitive demonstration audio.

## Tested Python environment

The versions pinned in [`requirements.txt`](requirements.txt) reflect the OCI Data Science Notebook Session in which the complete MVP workflow was successfully tested with Python 3.11.9 on x86_64. They document the validated reference environment; compatibility with other images, Python versions or architectures should be tested separately.

In that Notebook Session, `pip check` reported a pre-existing version mismatch: the OCI-provided `odsc-notebook-cli` 0.1.8 utility requires `jsonschema==4.5.1`, while the environment contained `jsonschema` 4.26.0. The application does not import or depend on `odsc-notebook-cli`, and its complete workflow executed successfully. `jsonschema` and `odsc-notebook-cli` are therefore intentionally not pinned in this repository. Avoid changing dependencies supplied by the OCI environment solely to suppress this warning, because doing so may affect other notebook components.

FFmpeg is a system executable rather than a Python package and is consequently not listed in `requirements.txt`. The tested environment used the x86_64 static FFmpeg 7.0.2 executable at `/usr/local/bin/ffmpeg`; installation and compatibility can vary with the selected OCI Data Science image and architecture.

This is an editable research MVP, not a production deployment architecture.

## License and disclaimer

This repository is released under the [MIT License](LICENSE). Read the full [Disclaimer](DISCLAIMER.md) before use. It is a personal, unsupported technical reference implementation provided as-is, without warranty, support or liability to the fullest extent permitted by law. The views expressed are personal and do not represent the author's employer or Oracle. Users remain responsible for their audio and transcription data, consent and data-rights obligations, security, costs, third-party terms and compliance obligations.
