# TTS API Reference

This document provides detailed information about the Text-to-Speech (TTS) API in MLX-Audio.

## Core Functions

### generate_audio

```python
from mlx_audio.tts.generate import generate_audio

generate_audio(
    text: str,
    model_path: str = "prince-canuma/Kokoro-82M",
    voice: str = "af_heart",
    speed: float = 1.0,
    lang_code: str = None,
    file_prefix: str = None,
    audio_format: str = "wav",
    sample_rate: int = 24000,
    join_audio: bool = True,
    verbose: bool = False
) -> str
```

Generates audio from text using the specified model and parameters.

#### Parameters

- `text` (str): The text to convert to speech
- `model_path` (str): Path to the model or model ID from Hugging Face
- `voice` (str): Voice style to use (default: "af_heart")
- `speed` (float): Speech speed multiplier (0.5 to 2.0)
- `lang_code` (str): Language code (e.g., 'a' for American English)
- `file_prefix` (str): Prefix for output file name
- `audio_format` (str): Output audio format (default: "wav")
- `sample_rate` (int): Audio sample rate (default: 24000)
- `join_audio` (bool): Whether to join multiple audio segments
- `verbose` (bool): Enable verbose output

#### Returns

- `str`: Path to the generated audio file

## Models

### KokoroPipeline

```python
from mlx_audio.tts.models.kokoro import KokoroPipeline

pipeline = KokoroPipeline(
    lang_code: str,
    model: Any,
    repo_id: str
)
```

Pipeline for the Kokoro TTS model.

#### Parameters

- `lang_code` (str): Language code
- `model`: Loaded model instance
- `repo_id` (str): Model repository ID

#### Methods

##### generate

```python
pipeline.generate(
    text: str,
    voice: str = "af_heart",
    speed: float = 1.0,
    split_pattern: str = r'\n+'
) -> Iterator[Tuple[str, str, np.ndarray]]
```

Generates audio from text.

#### Parameters

- `text` (str): Input text
- `voice` (str): Voice style
- `speed` (float): Speech speed
- `split_pattern` (str): Pattern to split text

#### Returns

- `Iterator[Tuple[str, str, np.ndarray]]`: Iterator of (text, voice, audio) tuples

## Utilities

### load_model

```python
from mlx_audio.tts.utils import load_model

model = load_model(repo_id: str) -> Any
```

Loads a model from Hugging Face.

#### Parameters

- `repo_id` (str): Model repository ID

#### Returns

- `Any`: Loaded model instance

### quantize_model

```python
from mlx_audio.tts.utils import quantize_model

weights, config = quantize_model(
    model: Any,
    config: Dict,
    group_size: int = 64,
    bits: int = 8
) -> Tuple[Dict, Dict]
```

Quantizes a model for improved performance.

#### Parameters

- `model`: Model to quantize
- `config`: Model configuration
- `group_size` (int): Group size for quantization
- `bits` (int): Number of bits for quantization

#### Returns

- `Tuple[Dict, Dict]`: Quantized weights and configuration

## Command Line Interface

The TTS functionality is also available through the command line:

```bash
mlx_audio.tts.generate [OPTIONS]

Options:
  --text TEXT           Text to convert to speech
  --model TEXT          Model path or ID
  --voice TEXT          Voice style
  --speed FLOAT        Speech speed
  --file_prefix TEXT   Output file prefix
  --audio_format TEXT  Output audio format
  --sample_rate INT    Audio sample rate
  --verbose           Enable verbose output
```

## Examples

### Basic Usage

```python
from mlx_audio.tts.generate import generate_audio

# Generate audio with default settings
generate_audio("Hello, world!")

# Generate audio with custom settings
generate_audio(
    text="Hello, world!",
    model_path="prince-canuma/Kokoro-82M",
    voice="af_heart",
    speed=1.2,
    lang_code="a"
)
```

### Advanced Usage

```python
from mlx_audio.tts.models.kokoro import KokoroPipeline
from mlx_audio.tts.utils import load_model, quantize_model

# Load and quantize model
model = load_model("prince-canuma/Kokoro-82M")
weights, config = quantize_model(model, model.config, bits=8)

# Create pipeline
pipeline = KokoroPipeline(lang_code="a", model=model, repo_id="prince-canuma/Kokoro-82M")

# Generate audio
for text, voice, audio in pipeline("Hello, world!", speed=1.2):
    print(f"Generated audio for: {text}")
``` 