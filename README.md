# QwenChat iOS

A native iOS chat application that runs Alibaba's [Qwen](https://github.com/QwenLM/Qwen) large language models **locally on-device** using Apple's [MLX](https://github.com/ml-explore/mlx) framework for GPU-accelerated inference.

## Features

- 💬 **On-Device AI Chat** – Run Qwen models entirely on your iPhone with no server or internet connection required during inference
- 🖼️ **Vision / Multimodal Support** – Attach photos from your library or camera and ask questions about images
- 🧠 **Thought Process Display** – Expandable "thinking" blocks show the model's reasoning (tagged `<think>`) before its final answer
- 🔄 **Model Switching** – Switch between Qwen 3.5 0.8B and 2B quantized models mid-conversation
- ⏹️ **Streaming with Stop Control** – Responses stream token-by-token and can be interrupted at any time
- 📊 **Performance Metrics** – Tokens-per-second displayed after each response

## Requirements

| Requirement | Version |
|-------------|---------|
| Xcode | 16.0+ |
| iOS Deployment Target | 18.0+ |
| Swift | 6.0 |
| Device | iPhone (physical device recommended; Apple Silicon Mac via Catalyst is untested) |

> **Note:** Models are downloaded from [Hugging Face Hub](https://huggingface.co/mlx-community) on first launch and cached locally. An internet connection is required for the initial model download.

## Getting Started

1. **Clone the repository**
   ```bash
   git clone https://github.com/andrisgauracs/qwen-chat-ios.git
   cd qwen-chat-ios
   ```

2. **Open in Xcode**
   ```bash
   open QwenChat.xcodeproj
   ```

3. **Resolve Swift packages**  
   Xcode will automatically fetch the [`mlx-swift-lm`](https://github.com/ml-explore/mlx-swift-lm) package dependency.

4. **Select a target device**  
   Choose a physical iPhone running iOS 18.0 or later.

5. **Build and run** (`⌘R`)

On first launch the app downloads the selected model from Hugging Face. Progress is shown in the UI. Subsequent launches use the cached model.

## Models

| Model | Hugging Face ID | Parameters | Vision |
|-------|-----------------|------------|--------|
| Qwen 3.5 0.8B (default) | `mlx-community/Qwen3.5-0.8B-4bit` | 0.8B | ✅ |
| Qwen 3.5 2B | `mlx-community/Qwen3.5-2B-4bit` | 2B | ✅ |

Models are 4-bit quantized for efficient on-device inference.

## Architecture

The project follows **MVVM** with SwiftUI:

```
QwenChat/
├── Models/
│   ├── ChatMessage.swift          # Message data structure (role, content, images, thinking blocks)
│   └── ModelConfiguration.swift  # Available model definitions with Hugging Face IDs
├── ViewModels/
│   └── ChatViewModel.swift        # Core logic: model loading, streaming generation, GPU management
├── Views/
│   ├── ContentView.swift          # Main chat list, model switcher menu, lifecycle handling
│   ├── ChatBubbleView.swift       # Individual message display with thinking block expansion
│   ├── MessageInputView.swift     # Text input, photo picker, send/stop button
│   └── ImagePreviewView.swift     # Thumbnail preview for attached images
└── QwenChatApp.swift              # App entry point
```

**Key dependencies:**
- [`mlx-swift-lm`](https://github.com/ml-explore/mlx-swift-lm) – Swift wrapper for LLM and vision-language model inference on Apple MLX
- `PhotosUI` – Native photo library and camera access
- `SwiftUI` + Swift 6 concurrency (`async`/`await`, `@Observable`)

## Entitlements

The current Xcode target does not use a custom entitlements file.

## License

This project does not currently include a license file. Please contact the repository owner for usage terms.
