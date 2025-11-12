# HeyGen Avatar IV API Code Examples

## Complete API Integration Examples for Music Videos

---

## Table of Contents

1. [Authentication](#authentication)
2. [Basic Music Video Generation](#basic-music-video-generation)
3. [Advanced Multi-Video Batch Processing](#advanced-multi-video-batch-processing)
4. [Error Handling & Retry Logic](#error-handling--retry-logic)
5. [Status Monitoring](#status-monitoring)
6. [Webhook Integration](#webhook-integration)
7. [Production-Ready Class](#production-ready-class)

---

## Authentication

### Get Your API Key

1. Log into HeyGen
2. Go to Settings → API Keys
3. Click "Create New API Key"
4. Copy and store securely

### Store Securely

**.env file**:
```bash
HEYGEN_API_KEY=your_api_key_here
```

**Python (.env)**:
```python
from dotenv import load_dotenv
import os

load_dotenv()
API_KEY = os.getenv('HEYGEN_API_KEY')
```

**Node.js (.env)**:
```javascript
require('dotenv').config();
const API_KEY = process.env.HEYGEN_API_KEY;
```

---

## Basic Music Video Generation

### Python Complete Example

```python
import requests
import time
import json
from pathlib import Path

class HeyGenAvatarIV:
    def __init__(self, api_key):
        self.api_key = api_key
        self.base_url = "https://api.heygen.com"
        self.headers = {
            "X-Api-Key": api_key,
            "Content-Type": "application/json"
        }

    def upload_image(self, image_path):
        """Upload avatar image and get image_key"""
        url = f"{self.base_url}/v2/photo_avatar/upload"

        with open(image_path, 'rb') as f:
            files = {'file': f}
            headers = {"X-Api-Key": self.api_key}
            response = requests.post(url, headers=headers, files=files)

        response.raise_for_status()
        return response.json()["data"]["image_key"]

    def upload_audio(self, audio_path):
        """Upload audio file and get audio_asset_id"""
        url = f"{self.base_url}/v2/assets/upload"

        with open(audio_path, 'rb') as f:
            files = {'file': f}
            headers = {"X-Api-Key": self.api_key}
            response = requests.post(url, headers=headers, files=files)

        response.raise_for_status()
        return response.json()["data"]["asset_id"]

    def create_video(self, image_key, audio_asset_id=None, audio_url=None,
                     script="Music performance", motion_prompt=None):
        """Generate Avatar IV video"""
        url = f"{self.base_url}/v2/video/av4/generate"

        payload = {
            "image_key": image_key,
            "video_title": "Music Video",
            "script": script,
            "voice_id": "en-US-Neural2-A",  # Required but overridden by audio
        }

        # Add audio (use asset_id or url)
        if audio_asset_id:
            payload["audio_asset_id"] = audio_asset_id
        elif audio_url:
            payload["audio_url"] = audio_url

        # Add motion customization
        if motion_prompt:
            payload["custom_motion_prompt"] = motion_prompt
            payload["enhance_custom_motion_prompt"] = True

        response = requests.post(url, headers=self.headers, json=payload)
        response.raise_for_status()
        return response.json()["data"]["video_id"]

    def check_status(self, video_id):
        """Check video generation status"""
        url = f"{self.base_url}/v2/video/av4/{video_id}/status"
        response = requests.get(url, headers=self.headers)
        response.raise_for_status()
        return response.json()["data"]

    def wait_for_completion(self, video_id, timeout=600, poll_interval=10):
        """Wait for video to complete and return URL"""
        start_time = time.time()

        while time.time() - start_time < timeout:
            status_data = self.check_status(video_id)

            if status_data["status"] == "completed":
                return status_data["video_url"]
            elif status_data["status"] == "failed":
                raise Exception(f"Video generation failed: {status_data}")

            print(f"Status: {status_data['status']}... waiting {poll_interval}s")
            time.sleep(poll_interval)

        raise TimeoutError(f"Video generation timed out after {timeout}s")

    def download_video(self, video_url, output_path):
        """Download completed video"""
        response = requests.get(video_url, stream=True)
        response.raise_for_status()

        with open(output_path, 'wb') as f:
            for chunk in response.iter_content(chunk_size=8192):
                f.write(chunk)

        print(f"Video downloaded to: {output_path}")


# Usage Example
def main():
    # Initialize
    api_key = "your_api_key_here"
    client = HeyGenAvatarIV(api_key)

    # Upload assets
    print("Uploading avatar image...")
    image_key = client.upload_image("./avatar.jpg")

    print("Uploading audio...")
    audio_asset_id = client.upload_audio("./song.mp3")

    # Create video with custom motion
    print("Creating video...")
    motion_prompt = "Gestures with hands, nods to beat, confident stance"
    video_id = client.create_video(
        image_key=image_key,
        audio_asset_id=audio_asset_id,
        motion_prompt=motion_prompt
    )

    # Wait for completion
    print(f"Video ID: {video_id}")
    print("Waiting for generation to complete...")
    video_url = client.wait_for_completion(video_id)

    # Download
    print("Downloading video...")
    client.download_video(video_url, "./output_music_video.mp4")

    print("Done!")


if __name__ == "__main__":
    main()
```

### Node.js Complete Example

```javascript
const axios = require('axios');
const FormData = require('form-data');
const fs = require('fs');
const { promisify } = require('util');
const sleep = promisify(setTimeout);

class HeyGenAvatarIV {
  constructor(apiKey) {
    this.apiKey = apiKey;
    this.baseUrl = 'https://api.heygen.com';
    this.headers = {
      'X-Api-Key': apiKey,
      'Content-Type': 'application/json'
    };
  }

  async uploadImage(imagePath) {
    const formData = new FormData();
    formData.append('file', fs.createReadStream(imagePath));

    const response = await axios.post(
      `${this.baseUrl}/v2/photo_avatar/upload`,
      formData,
      {
        headers: {
          'X-Api-Key': this.apiKey,
          ...formData.getHeaders()
        }
      }
    );

    return response.data.data.image_key;
  }

  async uploadAudio(audioPath) {
    const formData = new FormData();
    formData.append('file', fs.createReadStream(audioPath));

    const response = await axios.post(
      `${this.baseUrl}/v2/assets/upload`,
      formData,
      {
        headers: {
          'X-Api-Key': this.apiKey,
          ...formData.getHeaders()
        }
      }
    );

    return response.data.data.asset_id;
  }

  async createVideo(options) {
    const {
      imageKey,
      audioAssetId,
      audioUrl,
      script = 'Music performance',
      motionPrompt
    } = options;

    const payload = {
      image_key: imageKey,
      video_title: 'Music Video',
      script: script,
      voice_id: 'en-US-Neural2-A'
    };

    if (audioAssetId) {
      payload.audio_asset_id = audioAssetId;
    } else if (audioUrl) {
      payload.audio_url = audioUrl;
    }

    if (motionPrompt) {
      payload.custom_motion_prompt = motionPrompt;
      payload.enhance_custom_motion_prompt = true;
    }

    const response = await axios.post(
      `${this.baseUrl}/v2/video/av4/generate`,
      payload,
      { headers: this.headers }
    );

    return response.data.data.video_id;
  }

  async checkStatus(videoId) {
    const response = await axios.get(
      `${this.baseUrl}/v2/video/av4/${videoId}/status`,
      { headers: this.headers }
    );

    return response.data.data;
  }

  async waitForCompletion(videoId, timeout = 600000, pollInterval = 10000) {
    const startTime = Date.now();

    while (Date.now() - startTime < timeout) {
      const statusData = await this.checkStatus(videoId);

      if (statusData.status === 'completed') {
        return statusData.video_url;
      } else if (statusData.status === 'failed') {
        throw new Error(`Video generation failed: ${JSON.stringify(statusData)}`);
      }

      console.log(`Status: ${statusData.status}... waiting ${pollInterval/1000}s`);
      await sleep(pollInterval);
    }

    throw new Error(`Video generation timed out after ${timeout/1000}s`);
  }

  async downloadVideo(videoUrl, outputPath) {
    const response = await axios.get(videoUrl, { responseType: 'stream' });
    const writer = fs.createWriteStream(outputPath);

    response.data.pipe(writer);

    return new Promise((resolve, reject) => {
      writer.on('finish', () => {
        console.log(`Video downloaded to: ${outputPath}`);
        resolve();
      });
      writer.on('error', reject);
    });
  }
}

// Usage
async function main() {
  const apiKey = process.env.HEYGEN_API_KEY;
  const client = new HeyGenAvatarIV(apiKey);

  try {
    console.log('Uploading avatar image...');
    const imageKey = await client.uploadImage('./avatar.jpg');

    console.log('Uploading audio...');
    const audioAssetId = await client.uploadAudio('./song.mp3');

    console.log('Creating video...');
    const videoId = await client.createVideo({
      imageKey: imageKey,
      audioAssetId: audioAssetId,
      motionPrompt: 'Gestures with hands, nods to beat, confident stance'
    });

    console.log(`Video ID: ${videoId}`);
    console.log('Waiting for generation to complete...');
    const videoUrl = await client.waitForCompletion(videoId);

    console.log('Downloading video...');
    await client.downloadVideo(videoUrl, './output_music_video.mp4');

    console.log('Done!');
  } catch (error) {
    console.error('Error:', error.message);
  }
}

main();
```

---

## Advanced Multi-Video Batch Processing

### Python Batch Generator

```python
import asyncio
import aiohttp
from typing import List, Dict
import json

class BatchVideoGenerator:
    def __init__(self, api_key, max_concurrent=3):
        self.api_key = api_key
        self.base_url = "https://api.heygen.com"
        self.max_concurrent = max_concurrent
        self.headers = {
            "X-Api-Key": api_key,
            "Content-Type": "application/json"
        }

    async def create_video_async(self, session, config: Dict):
        """Create single video asynchronously"""
        url = f"{self.base_url}/v2/video/av4/generate"

        payload = {
            "image_key": config["image_key"],
            "video_title": config["title"],
            "script": config.get("script", "Performance"),
            "voice_id": "en-US-Neural2-A",
        }

        if "audio_asset_id" in config:
            payload["audio_asset_id"] = config["audio_asset_id"]
        elif "audio_url" in config:
            payload["audio_url"] = config["audio_url"]

        if "motion_prompt" in config:
            payload["custom_motion_prompt"] = config["motion_prompt"]
            payload["enhance_custom_motion_prompt"] = True

        async with session.post(url, json=payload, headers=self.headers) as response:
            data = await response.json()
            return {
                "config": config,
                "video_id": data["data"]["video_id"]
            }

    async def check_status_async(self, session, video_id):
        """Check status asynchronously"""
        url = f"{self.base_url}/v2/video/av4/{video_id}/status"

        async with session.get(url, headers=self.headers) as response:
            data = await response.json()
            return data["data"]

    async def wait_all(self, video_ids: List[str], timeout=600):
        """Wait for all videos to complete"""
        async with aiohttp.ClientSession() as session:
            start_time = asyncio.get_event_loop().time()
            completed = {}

            while len(completed) < len(video_ids):
                if asyncio.get_event_loop().time() - start_time > timeout:
                    raise TimeoutError("Batch generation timed out")

                # Check all pending videos
                pending = [vid for vid in video_ids if vid not in completed]
                tasks = [self.check_status_async(session, vid) for vid in pending]
                results = await asyncio.gather(*tasks)

                for video_id, status_data in zip(pending, results):
                    if status_data["status"] == "completed":
                        completed[video_id] = status_data["video_url"]
                        print(f"✓ Video {video_id} completed")
                    elif status_data["status"] == "failed":
                        completed[video_id] = None
                        print(f"✗ Video {video_id} failed")

                if len(completed) < len(video_ids):
                    await asyncio.sleep(10)

            return completed

    async def generate_batch(self, configs: List[Dict]):
        """Generate multiple videos in batch"""
        async with aiohttp.ClientSession() as session:
            # Create semaphore for concurrency control
            semaphore = asyncio.Semaphore(self.max_concurrent)

            async def create_with_limit(config):
                async with semaphore:
                    return await self.create_video_async(session, config)

            # Create all videos
            print(f"Creating {len(configs)} videos...")
            tasks = [create_with_limit(config) for config in configs]
            results = await asyncio.gather(*tasks)

            video_ids = [r["video_id"] for r in results]
            print(f"All videos created. Waiting for completion...")

            # Wait for all to complete
            completed = await self.wait_all(video_ids)

            return completed


# Usage
async def main():
    api_key = "your_api_key_here"
    generator = BatchVideoGenerator(api_key, max_concurrent=3)

    # Define multiple videos to create
    configs = [
        {
            "image_key": "image_key_1",
            "audio_url": "https://example.com/song1.mp3",
            "title": "Video 1",
            "motion_prompt": "Energetic gestures"
        },
        {
            "image_key": "image_key_2",
            "audio_url": "https://example.com/song2.mp3",
            "title": "Video 2",
            "motion_prompt": "Calm performance"
        },
        {
            "image_key": "image_key_3",
            "audio_url": "https://example.com/song3.mp3",
            "title": "Video 3",
            "motion_prompt": "Dancing movements"
        }
    ]

    # Generate all videos
    results = await generator.generate_batch(configs)

    # Print results
    for video_id, video_url in results.items():
        if video_url:
            print(f"✓ {video_id}: {video_url}")
        else:
            print(f"✗ {video_id}: Failed")


if __name__ == "__main__":
    asyncio.run(main())
```

---

## Error Handling & Retry Logic

### Robust Error Handling

```python
import requests
from requests.adapters import HTTPAdapter
from requests.packages.urllib3.util.retry import Retry
import time

class RobustHeyGenClient:
    def __init__(self, api_key):
        self.api_key = api_key
        self.base_url = "https://api.heygen.com"
        self.session = self._create_session()

    def _create_session(self):
        """Create session with retry logic"""
        session = requests.Session()

        retry_strategy = Retry(
            total=3,
            backoff_factor=2,
            status_forcelist=[429, 500, 502, 503, 504],
            allowed_methods=["GET", "POST"]
        )

        adapter = HTTPAdapter(max_retries=retry_strategy)
        session.mount("http://", adapter)
        session.mount("https://", adapter)

        return session

    def _make_request(self, method, endpoint, **kwargs):
        """Make API request with error handling"""
        url = f"{self.base_url}{endpoint}"
        headers = kwargs.pop("headers", {})
        headers["X-Api-Key"] = self.api_key

        max_attempts = 3
        attempt = 0

        while attempt < max_attempts:
            try:
                response = self.session.request(
                    method, url, headers=headers, timeout=30, **kwargs
                )

                # Handle rate limiting
                if response.status_code == 429:
                    retry_after = int(response.headers.get('Retry-After', 60))
                    print(f"Rate limited. Waiting {retry_after}s...")
                    time.sleep(retry_after)
                    attempt += 1
                    continue

                response.raise_for_status()
                return response.json()

            except requests.exceptions.Timeout:
                print(f"Request timeout. Attempt {attempt + 1}/{max_attempts}")
                attempt += 1
                time.sleep(2 ** attempt)

            except requests.exceptions.ConnectionError as e:
                print(f"Connection error: {e}. Retrying...")
                attempt += 1
                time.sleep(2 ** attempt)

            except requests.exceptions.HTTPError as e:
                # Don't retry client errors (4xx except 429)
                if 400 <= response.status_code < 500:
                    raise Exception(f"Client error: {response.status_code} - {response.text}")
                # Retry server errors
                else:
                    print(f"Server error. Retrying...")
                    attempt += 1
                    time.sleep(2 ** attempt)

        raise Exception(f"Failed after {max_attempts} attempts")

    def create_video(self, **kwargs):
        """Create video with error handling"""
        try:
            result = self._make_request(
                "POST",
                "/v2/video/av4/generate",
                json=kwargs
            )
            return result["data"]["video_id"]

        except Exception as e:
            print(f"Error creating video: {e}")
            raise

    def check_status_with_retry(self, video_id, max_retries=5):
        """Check status with exponential backoff"""
        for attempt in range(max_retries):
            try:
                result = self._make_request(
                    "GET",
                    f"/v2/video/av4/{video_id}/status"
                )
                return result["data"]

            except Exception as e:
                if attempt == max_retries - 1:
                    raise
                wait_time = 2 ** attempt
                print(f"Status check failed. Retrying in {wait_time}s...")
                time.sleep(wait_time)
```

---

## Status Monitoring

### Real-time Progress Monitor

```python
import time
from datetime import datetime, timedelta

class VideoProgressMonitor:
    def __init__(self, client):
        self.client = client

    def monitor_with_progress(self, video_id, estimated_duration=120):
        """Monitor with estimated progress"""
        start_time = time.time()
        last_status = None

        while True:
            status_data = self.client.check_status(video_id)
            current_status = status_data["status"]

            # Status changed
            if current_status != last_status:
                timestamp = datetime.now().strftime("%H:%M:%S")
                print(f"[{timestamp}] Status: {current_status}")
                last_status = current_status

            # Completed
            if current_status == "completed":
                elapsed = time.time() - start_time
                print(f"✓ Completed in {elapsed:.1f}s")
                return status_data["video_url"]

            # Failed
            if current_status == "failed":
                raise Exception("Video generation failed")

            # Show progress estimate
            elapsed = time.time() - start_time
            if current_status == "processing":
                progress = min(elapsed / estimated_duration * 100, 95)
                eta = estimated_duration - elapsed
                print(f"  Progress: ~{progress:.1f}% | ETA: {eta:.0f}s", end='\r')

            time.sleep(5)

# Usage
monitor = VideoProgressMonitor(client)
video_url = monitor.monitor_with_progress(video_id, estimated_duration=180)
```

---

## Webhook Integration

### Webhook Receiver (Flask)

```python
from flask import Flask, request, jsonify
import hmac
import hashlib

app = Flask(__name__)

# Store webhook secret from HeyGen dashboard
WEBHOOK_SECRET = "your_webhook_secret"

def verify_signature(payload, signature):
    """Verify webhook signature"""
    expected = hmac.new(
        WEBHOOK_SECRET.encode(),
        payload,
        hashlib.sha256
    ).hexdigest()

    return hmac.compare_digest(signature, expected)

@app.route('/webhook/heygen', methods=['POST'])
def heygen_webhook():
    """Handle HeyGen webhooks"""
    # Verify signature
    signature = request.headers.get('X-HeyGen-Signature')
    if not verify_signature(request.data, signature):
        return jsonify({"error": "Invalid signature"}), 401

    # Parse event
    event = request.json
    event_type = event.get('type')
    video_id = event.get('video_id')

    if event_type == 'video.completed':
        video_url = event.get('video_url')
        print(f"Video {video_id} completed: {video_url}")

        # Your custom logic here
        # - Download video
        # - Send notification
        # - Update database
        # - Trigger next step in workflow

    elif event_type == 'video.failed':
        error = event.get('error')
        print(f"Video {video_id} failed: {error}")

        # Handle failure
        # - Retry
        # - Alert admin
        # - Update status

    return jsonify({"status": "received"}), 200

if __name__ == '__main__':
    app.run(port=5000)
```

---

## Production-Ready Class

### Complete Production Implementation

```python
import requests
import time
import logging
from typing import Optional, Dict
from pathlib import Path
import json

# Setup logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

class HeyGenAvatarIVProduction:
    """Production-ready HeyGen Avatar IV client"""

    def __init__(self, api_key: str, timeout: int = 30):
        self.api_key = api_key
        self.base_url = "https://api.heygen.com"
        self.timeout = timeout
        self.session = self._create_session()

    def _create_session(self):
        """Create requests session with retry logic"""
        from requests.adapters import HTTPAdapter
        from requests.packages.urllib3.util.retry import Retry

        session = requests.Session()
        retry = Retry(
            total=3,
            backoff_factor=2,
            status_forcelist=[429, 500, 502, 503, 504]
        )
        adapter = HTTPAdapter(max_retries=retry)
        session.mount("http://", adapter)
        session.mount("https://", adapter)
        return session

    def _request(self, method: str, endpoint: str, **kwargs) -> Dict:
        """Make API request with error handling"""
        url = f"{self.base_url}{endpoint}"
        headers = kwargs.pop("headers", {})
        headers["X-Api-Key"] = self.api_key

        try:
            response = self.session.request(
                method, url, headers=headers,
                timeout=self.timeout, **kwargs
            )
            response.raise_for_status()
            return response.json()

        except requests.exceptions.RequestException as e:
            logger.error(f"API request failed: {e}")
            raise

    def upload_image(self, image_path: str) -> str:
        """Upload image and return image_key"""
        logger.info(f"Uploading image: {image_path}")

        with open(image_path, 'rb') as f:
            files = {'file': f}
            headers = {"X-Api-Key": self.api_key}
            response = self._request(
                "POST", "/v2/photo_avatar/upload",
                files=files, headers=headers
            )

        image_key = response["data"]["image_key"]
        logger.info(f"Image uploaded: {image_key}")
        return image_key

    def create_music_video(
        self,
        image_key: str,
        audio_url: str,
        title: str = "Music Video",
        motion_prompt: Optional[str] = None,
        quality: str = "higher_quality"
    ) -> str:
        """Create music video"""
        logger.info(f"Creating video: {title}")

        payload = {
            "image_key": image_key,
            "video_title": title,
            "script": "Music performance",
            "voice_id": "en-US-Neural2-A",
            "audio_url": audio_url,
            "quality": quality
        }

        if motion_prompt:
            payload["custom_motion_prompt"] = motion_prompt
            payload["enhance_custom_motion_prompt"] = True

        headers = {"Content-Type": "application/json"}
        response = self._request(
            "POST", "/v2/video/av4/generate",
            json=payload, headers=headers
        )

        video_id = response["data"]["video_id"]
        logger.info(f"Video created: {video_id}")
        return video_id

    def wait_for_video(
        self,
        video_id: str,
        timeout: int = 600,
        poll_interval: int = 10
    ) -> str:
        """Wait for video completion and return URL"""
        logger.info(f"Waiting for video: {video_id}")
        start_time = time.time()

        while time.time() - start_time < timeout:
            response = self._request(
                "GET", f"/v2/video/av4/{video_id}/status"
            )
            status_data = response["data"]
            status = status_data["status"]

            if status == "completed":
                video_url = status_data["video_url"]
                logger.info(f"Video completed: {video_url}")
                return video_url

            elif status == "failed":
                error = status_data.get("error", "Unknown error")
                logger.error(f"Video failed: {error}")
                raise Exception(f"Video generation failed: {error}")

            logger.info(f"Status: {status}, waiting {poll_interval}s...")
            time.sleep(poll_interval)

        raise TimeoutError(f"Video timed out after {timeout}s")

    def create_and_wait(
        self,
        image_path: str,
        audio_url: str,
        title: str = "Music Video",
        motion_prompt: Optional[str] = None,
        output_path: Optional[str] = None
    ) -> str:
        """Complete workflow: upload, create, wait, download"""

        # Upload image
        image_key = self.upload_image(image_path)

        # Create video
        video_id = self.create_music_video(
            image_key=image_key,
            audio_url=audio_url,
            title=title,
            motion_prompt=motion_prompt
        )

        # Wait for completion
        video_url = self.wait_for_video(video_id)

        # Download if output path provided
        if output_path:
            self.download_video(video_url, output_path)

        return video_url

    def download_video(self, video_url: str, output_path: str):
        """Download video to file"""
        logger.info(f"Downloading to: {output_path}")

        response = requests.get(video_url, stream=True)
        response.raise_for_status()

        Path(output_path).parent.mkdir(parents=True, exist_ok=True)

        with open(output_path, 'wb') as f:
            for chunk in response.iter_content(chunk_size=8192):
                f.write(chunk)

        logger.info(f"Downloaded: {output_path}")


# Usage Example
if __name__ == "__main__":
    # Initialize
    client = HeyGenAvatarIVProduction(
        api_key="your_api_key_here"
    )

    # Create music video
    try:
        video_url = client.create_and_wait(
            image_path="./avatar.jpg",
            audio_url="https://example.com/song.mp3",
            title="My Rap Video",
            motion_prompt="Gestures with hands, nods to beat",
            output_path="./output/music_video.mp4"
        )

        print(f"Success! Video URL: {video_url}")

    except Exception as e:
        logger.error(f"Failed: {e}")
        raise
```

---

## Testing & Debugging

### Test Script

```python
import unittest
from unittest.mock import Mock, patch

class TestHeyGenClient(unittest.TestCase):
    def setUp(self):
        self.client = HeyGenAvatarIVProduction("test_key")

    @patch('requests.Session.request')
    def test_upload_image(self, mock_request):
        # Mock response
        mock_response = Mock()
        mock_response.json.return_value = {
            "data": {"image_key": "test_key_123"}
        }
        mock_request.return_value = mock_response

        # Test
        image_key = self.client.upload_image("test.jpg")
        self.assertEqual(image_key, "test_key_123")

    @patch('requests.Session.request')
    def test_create_video(self, mock_request):
        mock_response = Mock()
        mock_response.json.return_value = {
            "data": {"video_id": "vid_123"}
        }
        mock_request.return_value = mock_response

        video_id = self.client.create_music_video(
            image_key="img_123",
            audio_url="https://example.com/song.mp3"
        )
        self.assertEqual(video_id, "vid_123")

if __name__ == "__main__":
    unittest.main()
```

---

**Ready to integrate Avatar IV into your application!**
