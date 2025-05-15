# Quickstart Guide

This guide will help you get started with MLX-Audio quickly.

## Basic Usage

### Command Line Interface

Generate audio from the command line:

```bash
# Basic usage
mlx_audio.tts.generate --text "Hello, world"

# Specify output file prefix
mlx_audio.tts.generate --text "Hello, world" --file_prefix hello

# Adjust speaking speed
mlx_audio.tts.generate --text "Hello, world" --speed 1.4
```

### Python API

Generate audio using the Python API:

```python
from mlx_audio.tts.generate import generate_audio

# Basic generation
generate_audio(
    text="Hello, world!",
    model_path="prince-canuma/Kokoro-82M",
    voice="af_heart",
    speed=1.2
)

# Advanced generation with more options
generate_audio(
    text="In the beginning, the universe was created...",
    model_path="prince-canuma/Kokoro-82M",
    voice="af_heart",
    speed=1.2,
    lang_code="a",  # American English
    file_prefix="audiobook_chapter1",
    audio_format="wav",
    sample_rate=24000,
    join_audio=True,
    verbose=True
)
```

## Web Interface

Start the web interface and API server:

```bash
# Start server
mlx_audio.server

# With custom host and port
mlx_audio.server --host 0.0.0.0 --port 9000

# With verbose logging
mlx_audio.server --verbose
```

Then open your browser and navigate to:
```
http://127.0.0.1:8000
```

## Using Different Models

### Kokoro Model

```python
from mlx_audio.tts.models.kokoro import KokoroPipeline
from mlx_audio.tts.utils import load_model
import soundfile as sf

# Initialize the model
model_id = 'prince-canuma/Kokoro-82M'
model = load_model(model_id)

# Create pipeline with American English
pipeline = KokoroPipeline(lang_code='a', model=model, repo_id=model_id)

# Generate audio
text = "The MLX King lives. Let him cook!"
for _, _, audio in pipeline(text, voice='af_heart', speed=1, split_pattern=r'\n+'):
    # Save audio to file
    sf.write('audio.wav', audio[0], 24000)
```

### CSM Model

```bash
# Generate speech using CSM-1B model with reference audio
python -m mlx_audio.tts.generate --model mlx-community/csm-1b --text "Hello from Sesame." --play --ref_audio ./conversational_a.wav
```

## Language Support

Kokoro model supports multiple languages:

- 🇺🇸 `'a'` - American English
- 🇬🇧 `'b'` - British English
- 🇯🇵 `'j'` - Japanese (requires `pip install misaki[ja]`)
- 🇨🇳 `'z'` - Mandarin Chinese (requires `pip install misaki[zh]`)

## Next Steps

- Check out the [API Reference](api/tts.md) for detailed documentation
- Explore [Advanced Features](examples/advanced-features.md) for more complex use cases
- Join our [Discord Community](https://discord.gg/mlx-audio) for support and updates 