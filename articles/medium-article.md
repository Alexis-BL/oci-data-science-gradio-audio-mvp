# Build an Interactive Audio Perception MVP with Gradio and OCI Data Science

## From a recorded sentence to word-level audio transformation and user feedback

Turning an audio-research hypothesis into a testable experience should not require building a full product first. This MVP combines Gradio, Whisper, Librosa, and SciPy in an OCI Data Science Notebook Session: an administrator records or uploads a sentence, obtains word-level timing, adjusts audio parameters, generates a transformed sequence, and sends it to an end user for a listening test.

OCI Data Science provides the managed JupyterLab workspace, selectable Compute, persistent storage, and simple start/stop lifecycle for the experiment. The scope is deliberately limited to a demonstrator. It is not a validated scientific protocol, clinical tool, hearing assessment, or production application.

> **Publication note:** the companion repository is currently private. Make it public, or replace the repository link with an appropriate public link, before publishing this article on Medium.

## What you will build

- Microphone recording or audio-file upload.
- French transcription with Whisper word timestamps.
- An editable word-level table for target frequency, bandwidth, and timing.
- Pitch shifting and Butterworth band-pass filtering for each word.
- Concatenated output audio, frequency plots, and an end-user transcription check.

Companion code: `notebooks/word-level-audio-perception-gradio-oci.ipynb` in the [repository](https://github.com/Alexis-BL/oci-data-science-gradio-audio-mvp).

## Prerequisites

You need an OCI account, permissions to create Data Science resources, a Notebook Session with outbound internet access, and the tutorial notebook. A CPU shape can run the MVP; a GPU shape may reduce transcription time. Run the notebook only with audio that you are permitted to process.

## Step 1 — Create the OCI workspace

1. In the OCI Console, open **Analytics & AI** → **Data Science** → **Projects**.
2. Create or select a project.
3. Create a Notebook Session with an appropriate VM shape, Block Storage, and outbound-capable network configuration.
4. Activate the session and open JupyterLab.
5. Keep notebooks and retained files on the attached Block Volume under `/home/datascience`.

## Step 2 — Upload and prepare the notebook

Upload `notebooks/word-level-audio-perception-gradio-oci.ipynb`, open it in JupyterLab, select a Python kernel, and run cells in their notebook order. Do not skip the installation cells.

## Step 3 — Install the dependencies and FFmpeg

Run the notebook installation cells:

```python
!pip install gradio
!pip install openai-whisper
!pip install gtts librosa soundfile
!pip install faster-whisper
!pip install ffmpeg-python
```

The notebook then downloads and installs FFmpeg, followed by:

```python
!ffmpeg -version
```

Whisper's `small` model is loaded when the application cell runs. First use can download model files and may take time depending on the selected shape and network.

## Step 4 — Transcribe a sentence

In the **Administrator** tab, record a French sentence or upload audio, then select **Transcribe**. The notebook invokes:

```python
res = _WHISPER_MODEL.transcribe(
    audio_path,
    language="fr",
    word_timestamps=True,
    fp16=False
)
```

The editable table contains the transcribed word, default target frequency of `1000 Hz`, default bandwidth of `500 Hz`, and the start/end timestamp. It also plots an estimated fundamental-frequency contour for the original audio.

## Step 5 — Adjust and validate word-level settings

Edit the target frequency, filter bandwidth, or timestamps when appropriate, then select **Validate table changes**. These values are experimental controls; the default target frequency is not an automatic per-word frequency measurement.

## Step 6 — Generate the audio sequence

Select **Generate concatenated audio**. For each word, the notebook extracts the segment, estimates fundamental frequency using `librosa.yin`, shifts its pitch toward the selected target using `librosa.effects.pitch_shift`, and applies a fourth-order Butterworth band-pass filter. The core sequence is:

```python
seg = y[i0:i1].astype(np.float32)
seg_shift = _shift_to_freq(seg, sr, target_freq_hz=f0)
seg_colored = _bandpass_filter(seg_shift, sr, fc_hz=f0, bw_hz=bw)
out.append(seg_colored)
```

The processed segments are concatenated with `np.concatenate(out)`. The application displays the result and its estimated fundamental-frequency contour.

## Step 7 — Send the test to an end user

Select **Send to user** and open the **End User** tab. The user listens to the generated audio, enters the phrase heard, and selects **Verify**. The MVP normalizes text and compares words with the administrator reference. This is a simple experimental check, not a sequence-aware accuracy metric or a scientific validation instrument.

## Step 8 — Share responsibly

The notebook calls:

```python
demo.launch(share=True)
```

This produces a convenient temporary Gradio demonstration link. Do not use it with confidential, personal, regulated, proprietary, or sensitive audio. It is not an OCI production endpoint and does not replace an application architecture with identity, authorization, secure hosting, monitoring, and data governance.

## From MVP to an OCI architecture

For an operational design, separate the user interface, audio storage, repeatable processing, and model-serving responsibilities. OCI Data Science Jobs can run repeatable tasks outside the notebook; Object Storage can retain inputs and outputs under controlled policies; and OCI Data Science Model Deployments provide managed HTTP endpoints for suitable model-serving workloads. These capabilities do not, by themselves, make a Gradio sharing URL a production deployment.

## Further resources

- [OCI Data Science overview](https://docs.oracle.com/en-us/iaas/data-science/using/overview.htm)
- [Notebook Sessions](https://docs.oracle.com/iaas/Content/data-science/using/use-notebook-sessions.htm)
- [OCI Data Science Jobs](https://docs.oracle.com/en-us/iaas/Content/GSG/Reference/getting-started-as-data-scientist.htm)
- [OCI Data Science Model Deployments](https://docs.oracle.com/en-us/iaas/Content/data-science/using/ai-quick-actions-model-deploy.htm)
- [Companion repository disclaimer](../DISCLAIMER.md)

## Disclaimer

This article and its companion repository are a personal technical reference implementation and starter MVP, provided for education, experimentation, and evaluation only. They are not a production-ready product, managed service, security or compliance solution, legal, medical, scientific, financial, or professional advice, or a commitment to provide any service. This MVP is not a validated scientific protocol, clinical tool, hearing assessment, speech-recognition guarantee, or safety-critical system. Its audio transformation and text-verification results may be incomplete, inaccurate, version-dependent, or unsuitable for a particular use; independent validation and human review are required before consequential use.

The views, technical perspectives, and recommendations are personal. They do not represent the views, policies, positions, recommendations, or commitments of the author's employer, Oracle, Oracle Cloud Infrastructure, or any affiliated entity. This is not an official Oracle publication, product, reference architecture, support statement, certification, or endorsement.

All material is provided **AS IS** and **AS AVAILABLE**, without warranty or condition of any kind, express or implied, to the fullest extent permitted by law. No guarantee is made as to correctness, availability, performance, security, reliability, transcription quality, word timestamps, pitch estimation, audio intelligibility, reproducibility, scientific validity, cost, fitness for purpose, non-infringement, or suitability. No support, SLA, maintenance, incident response, security remediation, training, troubleshooting, compatibility, or production commitment is provided. Users are solely responsible for review, testing, adaptation, operation, security, monitoring, backup, and compliance.

Audio recordings and transcriptions can contain personal, biometric, sensitive, confidential, proprietary, or third-party content. Do not process, upload, share, or publish them without the necessary rights, notices, permissions, and legal basis. The notebook can download packages and models and can expose a temporary Gradio sharing link. Do not use that link with confidential, personal, regulated, proprietary, or sensitive audio. Users must independently assess provider terms, data residency, retention, consent, copyright, contractual terms, encryption, IAM, network access, and regulatory obligations before sending data to OCI, model providers, or other third-party services. Never commit credentials, private keys, tokens, PAR URLs, confidential recordings, personal data, or proprietary outputs to source control, screenshots, issues, logs, or prompts.

OCI Compute, Block Storage, Object Storage, network traffic, and third-party services may incur charges. Always Free eligibility, capacity, quotas, service limits, pricing, regions, APIs, package behavior, and model availability may change. The MIT License applies only to material authored in the companion repository; it grants no right to third-party services, audio, documentation, or marks. Oracle, OCI, Gradio, OpenAI, Whisper, Hugging Face, Librosa, SciPy, FFmpeg, Jupyter, Python, and all other names and marks belong to their respective owners; use is descriptive only and does not imply affiliation, sponsorship, certification, or endorsement.

To the fullest extent permitted by law, the author and copyright holders are not liable for direct, indirect, incidental, special, consequential, exemplary, or other loss or damage, including loss of data, audio, revenue, business, reputation, availability, security, or anticipated savings, arising from or related to this article, the repository, their use, inability to use them, or any configured service or provider. Nothing excludes liability that cannot lawfully be excluded or limited. By using the material, you accept responsibility for your own review, decisions, deployment, and compliance obligations.
