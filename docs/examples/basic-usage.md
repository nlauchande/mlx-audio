# Basic Usage Examples

This guide provides basic examples of using MLX-Audio for text-to-speech generation.

## Command Line Usage

### Basic Text-to-Speech

```bash
# Generate audio with default settings
mlx_audio.tts.generate --text "Hello, world!"

# Specify output file prefix
mlx_audio.tts.generate --text "Hello, world!" --file_prefix hello

# Adjust speaking speed
mlx_audio.tts.generate --text "Hello, world!" --speed 1.4
```

### Using Different Voices

```bash
# Use American English voice
mlx_audio.tts.generate --text "Hello, world!" --voice af_heart

# Use British English voice
mlx_audio.tts.generate --text "Hello, world!" --voice bf_emma
```

### Saving to Different Formats

```bash
# Save as WAV file
mlx_audio.tts.generate --text "Hello, world!" --audio_format wav

# Save as MP3 file
mlx_audio.tts.generate --text "Hello, world!" --audio_format mp3
```

## Python API Usage

### Basic Generation

```python
from mlx_audio.tts.generate import generate_audio

# Generate audio with default settings
generate_audio("Hello, world!")

# Generate audio with custom settings
generate_audio(
    text="Hello, world!",
    model_path="prince-canuma/Kokoro-82M",
    voice="af_heart",
    speed=1.2
)
```

### Using Different Languages

```python
from mlx_audio.tts.generate import generate_audio

# American English
generate_audio(
    text="Hello, world!",
    lang_code="a"
)

# British English
generate_audio(
    text="Hello, world!",
    lang_code="b"
)

# Japanese
generate_audio(
    text="こんにちは、世界！",
    lang_code="j"
)

# Mandarin Chinese
generate_audio(
    text="你好，世界！",
    lang_code="z"
)
```

### Saving Audio Files

```python
from mlx_audio.tts.generate import generate_audio

# Save with custom prefix
generate_audio(
    text="Hello, world!",
    file_prefix="greeting"
)

# Save in different format
generate_audio(
    text="Hello, world!",
    audio_format="mp3"
)

# Save with custom sample rate
generate_audio(
    text="Hello, world!",
    sample_rate=44100
)
```

## Web Interface Usage

### Starting the Server

```bash
# Start with default settings
mlx_audio.server

# Start on specific port
mlx_audio.server --port 9000

# Start with verbose logging
mlx_audio.server --verbose
```

### Using the Web Interface

1. Open your browser and navigate to `http://localhost:8000`
2. Enter your text in the input field
3. Select voice and speed settings
4. Click "Generate" to create audio
5. Use the 3D visualization to see audio frequencies
6. Download or play the generated audio

## Common Use Cases

### Generating an Audiobook Chapter

```python
from mlx_audio.tts.generate import generate_audio

# Generate an audiobook chapter
generate_audio(
    text=("In the beginning, the universe was created...\n"
          "...or the simulation was booted up."),
    model_path="prince-canuma/Kokoro-82M",
    voice="af_heart",
    speed=1.2,
    lang_code="a",
    file_prefix="audiobook_chapter1",
    audio_format="wav",
    sample_rate=24000,
    join_audio=True,
    verbose=True
)
```

### Creating a Multilingual Greeting

```python
from mlx_audio.tts.generate import generate_audio

# Generate greetings in different languages
greetings = {
    "a": "Hello, welcome to our service!",
    "b": "Hello, welcome to our service!",
    "j": "ようこそ、私たちのサービスへ！",
    "z": "欢迎使用我们的服务！"
}

for lang_code, text in greetings.items():
    generate_audio(
        text=text,
        lang_code=lang_code,
        file_prefix=f"greeting_{lang_code}"
    )
```

### Batch Processing

```python
from mlx_audio.tts.generate import generate_audio

# Process multiple texts
texts = [
    "Welcome to our service.",
    "Thank you for choosing us.",
    "We hope you enjoy your experience."
]

for i, text in enumerate(texts, 1):
    generate_audio(
        text=text,
        file_prefix=f"message_{i}",
        speed=1.1
    )
```

## Next Steps

- Check out [Advanced Features](advanced-features.md) for more complex use cases
- Explore the [API Reference](../api/tts.md) for detailed documentation
- Learn about [Model Configuration](../api/models.md) for customization 