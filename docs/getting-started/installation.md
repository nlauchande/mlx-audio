# Installation Guide

This guide will help you set up MLX-Audio on your system.

## Prerequisites

- Python 3.8 or higher
- Apple Silicon Mac (M1/M2/M3 series)
- pip (Python package installer)

## Basic Installation

Install MLX-Audio using pip:

```bash
pip install mlx-audio
```

## Full Installation with Web Interface

For the complete installation including the web interface and API server:

```bash
# Clone the repository
git clone https://github.com/yourusername/mlx-audio.git
cd mlx-audio

# Install the package and dependencies
pip install -r requirements.txt
```

## System Requirements

### Hardware
- Apple Silicon Mac (M1/M2/M3 series)
- Minimum 8GB RAM (16GB recommended)
- 2GB free disk space

### Software
- macOS 12.0 or later
- Python 3.8 or higher
- pip (Python package installer)

## Optional Dependencies

### Language Support
For additional language support:

```bash
# Japanese support
pip install misaki[ja]

# Chinese support
pip install misaki[zh]
```

### Development Dependencies
For development and testing:

```bash
pip install -r requirements-dev.txt
```

## Verifying Installation

To verify your installation:

```python
from mlx_audio.tts.generate import generate_audio

# Test generation
generate_audio(
    text="Hello, world!",
    model_path="prince-canuma/Kokoro-82M",
    voice="af_heart"
)
```

## Troubleshooting

### Common Issues

1. **MLX Installation Fails**
   - Ensure you're using Python 3.8 or higher
   - Try installing MLX separately: `pip install mlx`

2. **Missing Dependencies**
   - Run `pip install -r requirements.txt` to install all dependencies
   - Check the error message for specific missing packages

3. **Performance Issues**
   - Ensure you're running on Apple Silicon
   - Check system resources (CPU, memory usage)

### Getting Help

If you encounter any issues:
- Check the [GitHub Issues](https://github.com/yourusername/mlx-audio/issues)
- Join our [Discord Community](https://discord.gg/mlx-audio)
- Open a new issue with detailed information about your problem 