# MLX-Audio

A text-to-speech (TTS) and Speech-to-Speech (STS) library built on Apple's MLX framework, providing efficient speech synthesis on Apple Silicon.

## Features

- 🚀 **Fast Inference**: Optimized for Apple Silicon (M series chips)
- 🌍 **Multiple Languages**: Support for various languages and voice styles
- 🎛️ **Voice Customization**: Flexible voice options and parameters
- ⚡ **Speed Control**: Adjustable speech speed (0.5x to 2.0x)
- 🎨 **Interactive Web Interface**: 3D audio visualization
- 🔌 **REST API**: Easy integration with your applications
- 📊 **Quantization Support**: Optimized performance through model quantization
- 📂 **File System Integration**: Direct access to output files

## Quick Start

```python
from mlx_audio.tts.generate import generate_audio

# Generate audio with default settings
generate_audio(
    text="Hello, world!",
    model_path="prince-canuma/Kokoro-82M",
    voice="af_heart",
    speed=1.2
)
```

## Installation

```bash
pip install mlx-audio
```

For web interface and API dependencies:
```bash
pip install -r requirements.txt
```

## Documentation

- [Installation Guide](getting-started/installation.md)
- [Quickstart Guide](getting-started/quickstart.md)
- [API Reference](api/tts.md)
- [Examples](examples/basic-usage.md)

## Requirements

- MLX
- Python 3.8+
- Apple Silicon Mac (for optimal performance)

## License

[MIT License](LICENSE)

## Acknowledgements

- Thanks to the Apple MLX team for providing a great framework
- This project uses the Kokoro model architecture for text-to-speech synthesis
- The 3D visualization uses Three.js for rendering 