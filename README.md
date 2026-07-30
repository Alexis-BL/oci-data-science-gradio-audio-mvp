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

## Contents

- `notebooks/word-level-audio-perception-gradio-oci.ipynb` — tutorial notebook (to be added from the source file)
- `data/` — optional local test audio; not committed by default

## Run in OCI Data Science

1. Create and activate an OCI Data Science Notebook Session with outbound internet access.
2. Upload the notebook to JupyterLab.
3. Run the cells in order, including package and FFmpeg installation.
4. Open the Gradio link emitted by `demo.launch(share=True)` only for non-sensitive demonstration audio.

This is an editable research MVP, not a production deployment architecture.

## License and disclaimer

This repository is released under the [MIT License](LICENSE). Read the full [Disclaimer](DISCLAIMER.md) before use. It is a personal, unsupported technical reference implementation provided as-is, without warranty, support or liability to the fullest extent permitted by law. The views expressed are personal and do not represent the author's employer or Oracle. Users remain responsible for their audio and transcription data, consent and data-rights obligations, security, costs, third-party terms and compliance obligations.
