# Models API Reference

This document provides detailed information about the available models in MLX-Audio.

## Available Models

### Kokoro

The Kokoro model is a multilingual TTS model that supports various languages and voice styles.

#### Model Details

- **Repository**: `prince-canuma/Kokoro-82M`
- **Size**: 82M parameters
- **Languages**: American English, British English, Japanese, Mandarin Chinese
- **Voice Styles**: af_heart, af_nova, af_bella, bf_emma

#### Usage

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

#### Language Codes

- 🇺🇸 `'a'` - American English
- 🇬🇧 `'b'` - British English
- 🇯🇵 `'j'` - Japanese (requires `pip install misaki[ja]`)
- 🇨🇳 `'z'` - Mandarin Chinese (requires `pip install misaki[zh]`)

### CSM (Conversational Speech Model)

The CSM model allows for text-to-speech and voice customization using reference audio samples.

#### Model Details

- **Repository**: `mlx-community/csm-1b`
- **Size**: 1B parameters
- **Features**: Voice cloning, reference audio support

#### Usage

```python
from mlx_audio.tts.models.csm import CSMPipeline
from mlx_audio.tts.utils import load_model

# Initialize the model
model_id = 'mlx-community/csm-1b'
model = load_model(model_id)

# Create pipeline
pipeline = CSMPipeline(model=model, repo_id=model_id)

# Generate audio with reference
audio = pipeline.generate(
    text="Hello from Sesame.",
    ref_audio="./conversational_a.wav",
    speed=1.0
)
```

#### Command Line Usage

```bash
# Generate speech using CSM-1B model with reference audio
python -m mlx_audio.tts.generate --model mlx-community/csm-1b --text "Hello from Sesame." --play --ref_audio ./conversational_a.wav
```

## Model Management

### Loading Models

```python
from mlx_audio.tts.utils import load_model

# Load a model
model = load_model("prince-canuma/Kokoro-82M")
```

### Model Quantization

```python
from mlx_audio.tts.utils import quantize_model
import mlx.core as mx

# Load model
model = load_model("prince-canuma/Kokoro-82M")
config = model.config

# Quantize to 8-bit
weights, config = quantize_model(model, config, group_size=64, bits=8)

# Save quantized model
mx.save_safetensors("./8bit/model.safetensors", weights, metadata={"format": "mlx"})
```

## Model Configuration

### Kokoro Configuration

```python
{
    "model_type": "kokoro",
    "vocab_size": 256,
    "hidden_size": 512,
    "num_hidden_layers": 12,
    "num_attention_heads": 8,
    "intermediate_size": 2048,
    "max_position_embeddings": 1024,
    "layer_norm_eps": 1e-5,
    "pad_token_id": 0,
    "bos_token_id": 1,
    "eos_token_id": 2
}
```

### CSM Configuration

```python
{
    "model_type": "csm",
    "vocab_size": 256,
    "hidden_size": 1024,
    "num_hidden_layers": 24,
    "num_attention_heads": 16,
    "intermediate_size": 4096,
    "max_position_embeddings": 2048,
    "layer_norm_eps": 1e-5,
    "pad_token_id": 0,
    "bos_token_id": 1,
    "eos_token_id": 2
}
```

## Performance Considerations

### Memory Usage

- Kokoro (82M): ~500MB RAM
- CSM (1B): ~2GB RAM

### Quantization Benefits

- 8-bit quantization reduces memory usage by ~75%
- Minimal impact on audio quality
- Faster inference times

### Best Practices

1. Use quantization for production deployments
2. Monitor memory usage with large models
3. Consider batch processing for multiple generations
4. Use appropriate language codes for optimal results 