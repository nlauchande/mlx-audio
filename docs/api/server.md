# Server API Reference

This document provides detailed information about the web interface and API server in MLX-Audio.

## Web Interface

The web interface provides a user-friendly way to generate and visualize audio with a 3D visualization that reacts to audio frequencies.

### Starting the Server

```bash
# Basic usage
mlx_audio.server

# With custom host and port
mlx_audio.server --host 0.0.0.0 --port 9000

# With verbose logging
mlx_audio.server --verbose
```

### Features

- **Multiple Voice Options**: Choose from different voice styles
- **Adjustable Speech Speed**: Control the speed of speech generation
- **Real-time 3D Visualization**: Responsive 3D orb that reacts to audio frequencies
- **Audio Upload**: Play and visualize your own audio files
- **Auto-play Option**: Automatically play generated audio
- **Output Folder Access**: Open the output folder directly from the interface

## REST API

The server provides a REST API for programmatic access to TTS functionality.

### Endpoints

#### Generate TTS

```http
POST /tts
Content-Type: application/x-www-form-urlencoded

text=Hello, world!&voice=af_heart&speed=1.2
```

**Parameters:**
- `text` (required): The text to convert to speech
- `voice` (optional): Voice style (default: "af_heart")
- `speed` (optional): Speech speed from 0.5 to 2.0 (default: 1.0)

**Response:**
```json
{
    "filename": "output_1234567890.wav",
    "status": "success"
}
```

#### Get Audio File

```http
GET /audio/{filename}
```

**Parameters:**
- `filename`: The filename of the audio to retrieve

**Response:**
- Audio file in the requested format

#### Play Audio

```http
POST /play
Content-Type: application/x-www-form-urlencoded

filename=output_1234567890.wav
```

**Parameters:**
- `filename` (required): The filename of the audio to play

**Response:**
```json
{
    "status": "success",
    "filename": "output_1234567890.wav"
}
```

#### Stop Audio

```http
POST /stop
```

**Response:**
```json
{
    "status": "success"
}
```

#### Open Output Folder

```http
POST /open_output_folder
```

**Response:**
```json
{
    "status": "success",
    "path": "/path/to/output/folder"
}
```

## Python API

### Server Class

```python
from mlx_audio.server import Server

server = Server(
    host: str = "127.0.0.1",
    port: int = 8000,
    verbose: bool = False
)
```

#### Parameters

- `host` (str): Host address to bind the server to
- `port` (int): Port to bind the server to
- `verbose` (bool): Enable verbose logging

#### Methods

##### start

```python
server.start() -> None
```

Starts the server.

##### stop

```python
server.stop() -> None
```

Stops the server.

## Configuration

### Server Configuration

```python
{
    "host": "127.0.0.1",
    "port": 8000,
    "verbose": False,
    "output_dir": "~/.mlx_audio/outputs",
    "max_file_size": 10485760,  # 10MB
    "allowed_extensions": [".wav", ".mp3", ".ogg"]
}
```

### Web Interface Configuration

```python
{
    "theme": "light",
    "default_voice": "af_heart",
    "default_speed": 1.0,
    "auto_play": False,
    "visualization": {
        "enabled": True,
        "sensitivity": 1.0,
        "color": "#4CAF50"
    }
}
```

## Security Considerations

1. **File Size Limits**
   - Maximum file size: 10MB
   - Allowed extensions: .wav, .mp3, .ogg

2. **Access Control**
   - Default: localhost only
   - For remote access: Use `--host 0.0.0.0` with proper firewall rules

3. **Output Directory**
   - Default: `~/.mlx_audio/outputs`
   - Fallback to system temp directory if not writable

## Error Handling

### Common Error Responses

```json
{
    "error": "Invalid parameters",
    "details": "Text parameter is required"
}
```

```json
{
    "error": "File not found",
    "details": "output_1234567890.wav does not exist"
}
```

```json
{
    "error": "Server error",
    "details": "Internal server error occurred"
}
```

## Examples

### Python Client

```python
import requests

# Generate TTS
response = requests.post(
    "http://localhost:8000/tts",
    data={
        "text": "Hello, world!",
        "voice": "af_heart",
        "speed": 1.2
    }
)
filename = response.json()["filename"]

# Play the generated audio
requests.post(
    "http://localhost:8000/play",
    data={"filename": filename}
)
```

### cURL Examples

```bash
# Generate TTS
curl -X POST http://localhost:8000/tts \
  -d "text=Hello, world!&voice=af_heart&speed=1.2"

# Play audio
curl -X POST http://localhost:8000/play \
  -d "filename=output_1234567890.wav"

# Stop audio
curl -X POST http://localhost:8000/stop
``` 