# Advanced Features

This guide covers advanced features and use cases of MLX-Audio.

## Model Quantization

Quantization reduces model size and improves inference speed with minimal impact on quality.

### Basic Quantization

```python
from mlx_audio.tts.utils import quantize_model, load_model
import mlx.core as mx

# Load model
model = load_model("prince-canuma/Kokoro-82M")
config = model.config

# Quantize to 8-bit
weights, config = quantize_model(model, config, group_size=64, bits=8)

# Save quantized model
mx.save_safetensors("./8bit/model.safetensors", weights, metadata={"format": "mlx"})
```

### Custom Quantization

```python
from mlx_audio.tts.utils import quantize_model, load_model
import mlx.core as mx

# Load model
model = load_model("prince-canuma/Kokoro-82M")
config = model.config

# Custom quantization settings
group_size = 128
bits = 4  # More aggressive quantization

# Quantize model
weights, config = quantize_model(
    model,
    config,
    group_size=group_size,
    bits=bits
)

# Save with custom metadata
mx.save_safetensors(
    "./4bit/model.safetensors",
    weights,
    metadata={
        "format": "mlx",
        "quantization": {
            "bits": bits,
            "group_size": group_size
        }
    }
)
```

## Voice Cloning with CSM

The CSM model allows for voice cloning using reference audio.

### Basic Voice Cloning

```python
from mlx_audio.tts.models.csm import CSMPipeline
from mlx_audio.tts.utils import load_model

# Initialize model
model = load_model("mlx-community/csm-1b")
pipeline = CSMPipeline(model=model, repo_id="mlx-community/csm-1b")

# Generate audio with reference
audio = pipeline.generate(
    text="Hello, this is a cloned voice.",
    ref_audio="./reference.wav",
    speed=1.0
)
```

### Batch Voice Cloning

```python
from mlx_audio.tts.models.csm import CSMPipeline
from mlx_audio.tts.utils import load_model
import soundfile as sf

# Initialize model
model = load_model("mlx-community/csm-1b")
pipeline = CSMPipeline(model=model, repo_id="mlx-community/csm-1b")

# Multiple texts with same voice
texts = [
    "First message in cloned voice.",
    "Second message in cloned voice.",
    "Third message in cloned voice."
]

# Generate all texts with same reference
for i, text in enumerate(texts, 1):
    audio = pipeline.generate(
        text=text,
        ref_audio="./reference.wav",
        speed=1.0
    )
    sf.write(f"cloned_voice_{i}.wav", audio[0], 24000)
```

## Custom Model Training

### Fine-tuning Kokoro

```python
from mlx_audio.tts.models.kokoro import KokoroPipeline
from mlx_audio.tts.utils import load_model
import mlx.core as mx

# Load base model
model = load_model("prince-canuma/Kokoro-82M")
config = model.config

# Prepare training data
training_data = [
    ("Hello, world!", "af_heart"),
    ("How are you?", "af_heart"),
    # Add more training examples
]

# Fine-tune model
for text, voice in training_data:
    # Training logic here
    pass

# Save fine-tuned model
mx.save_safetensors("./fine_tuned/model.safetensors", model.weights)
```

## Advanced Audio Processing

### Audio Post-processing

```python
from mlx_audio.tts.generate import generate_audio
import soundfile as sf
import numpy as np
from scipy import signal

# Generate base audio
generate_audio(
    text="Hello, world!",
    file_prefix="base_audio"
)

# Load and process audio
audio, sample_rate = sf.read("base_audio.wav")

# Apply effects
# 1. Normalize
audio = audio / np.max(np.abs(audio))

# 2. Apply low-pass filter
b, a = signal.butter(4, 1000/(sample_rate/2), 'low')
filtered_audio = signal.filtfilt(b, a, audio)

# 3. Add reverb (simplified)
reverb = np.random.randn(len(audio)) * 0.1
processed_audio = filtered_audio + reverb

# Save processed audio
sf.write("processed_audio.wav", processed_audio, sample_rate)
```

### Batch Processing with Post-processing

```python
from mlx_audio.tts.generate import generate_audio
import soundfile as sf
import numpy as np
from scipy import signal
import os

def process_audio(input_file, output_file):
    # Load audio
    audio, sample_rate = sf.read(input_file)
    
    # Apply processing
    # 1. Normalize
    audio = audio / np.max(np.abs(audio))
    
    # 2. Apply effects
    b, a = signal.butter(4, 1000/(sample_rate/2), 'low')
    filtered_audio = signal.filtfilt(b, a, audio)
    
    # 3. Save processed audio
    sf.write(output_file, filtered_audio, sample_rate)

# Generate and process multiple files
texts = [
    "First message.",
    "Second message.",
    "Third message."
]

for i, text in enumerate(texts, 1):
    # Generate audio
    generate_audio(
        text=text,
        file_prefix=f"message_{i}"
    )
    
    # Process audio
    process_audio(
        f"message_{i}.wav",
        f"processed_message_{i}.wav"
    )
```

## Performance Optimization

### Batch Processing with Parallel Execution

```python
from mlx_audio.tts.generate import generate_audio
import concurrent.futures
import os

def generate_audio_file(text, index):
    return generate_audio(
        text=text,
        file_prefix=f"batch_{index}",
        verbose=False
    )

# List of texts to process
texts = [
    "First message.",
    "Second message.",
    "Third message.",
    "Fourth message.",
    "Fifth message."
]

# Process in parallel
with concurrent.futures.ThreadPoolExecutor(max_workers=4) as executor:
    futures = [
        executor.submit(generate_audio_file, text, i)
        for i, text in enumerate(texts)
    ]
    
    # Wait for all tasks to complete
    for future in concurrent.futures.as_completed(futures):
        try:
            result = future.result()
            print(f"Generated: {result}")
        except Exception as e:
            print(f"Error: {e}")
```

### Memory Management

```python
from mlx_audio.tts.generate import generate_audio
import gc
import mlx.core as mx

def generate_with_memory_management(text, index):
    try:
        # Generate audio
        result = generate_audio(
            text=text,
            file_prefix=f"memory_managed_{index}"
        )
        
        # Clear MLX cache
        mx.clear_cache()
        
        # Force garbage collection
        gc.collect()
        
        return result
    except Exception as e:
        print(f"Error in generation {index}: {e}")
        return None

# Process multiple texts with memory management
texts = [
    "First long message...",
    "Second long message...",
    "Third long message..."
]

for i, text in enumerate(texts):
    result = generate_with_memory_management(text, i)
    if result:
        print(f"Successfully generated {result}")
```

## Next Steps

- Explore the [API Reference](../api/tts.md) for more details
- Check out [Model Configuration](../api/models.md) for customization options
- Join our [Discord Community](https://discord.gg/mlx-audio) for support and updates 