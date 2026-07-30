# Word-Level Audio Perception MVP on OCI Data Science

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
