# Vitalis AI Services


A suite of advanced AI microservices designed to enhance hospital management systems with intelligent medical image analysis, document processing, and clinical decision support capabilities.

## Table of Contents

- [Overview](#overview)
- [Microservices](#microservices)
  - [TB-Detection API](#tb-detection-api)
  - [AI Image Doctor](#ai-image-doctor)
  - [AI Assistant Doctor](#ai-assistant-doctor)
  - [Chat Microservice](#chat-microservice)
- [Installation](#installation)
- [Configuration](#configuration)
- [API Documentation](#api-documentation)
- [Integration Guide](#integration-guide)
- [License](#license)

## Overview

Vitalis AI Services is a comprehensive suite of microservices designed to augment hospital management systems with state-of-the-art artificial intelligence capabilities. Each microservice focuses on a specific area of healthcare AI, providing specialized functionality while maintaining a consistent integration interface.

This suite enables healthcare providers to:
- Analyze medical images with advanced AI models
- Extract and process information from medical documents
- Provide intelligent clinical decision support
- Facilitate natural language interaction with medical knowledge

## Microservices

### TB-Detection API

A specialized microservice for automated tuberculosis detection from chest X-rays using deep learning.

**Key Features:**
- Automated TB detection from chest X-ray images
- Confidence scoring with probability assessment
- AI-generated radiological interpretations
- Follow-up query capabilities for specific medical questions

**Technical Stack:**
- TensorFlow/Keras for deep learning
- Ollama for local LLM-based report generation
- FastAPI for API endpoints

**Server:** Runs on port 8000

### AI Image Doctor

A comprehensive medical image analysis service that leverages GPT-4 vision capabilities to analyze various types of medical images.

**Key Features:**
- Analysis of various medical imaging modalities (X-ray, CT, MRI)
- Advanced pattern recognition for anomaly detection
- Anatomical structure identification
- Comprehensive pathology assessment
- Reference-based image comparison

**Technical Stack:**
- OpenAI GPT-4 with vision capabilities
- FastEmbed for efficient image embeddings 
- Qdrant for vector similarity search
- FastAPI for robust API endpoints

**Server:** Runs on port 8001

### AI Assistant Doctor

An intelligent document processing service for medical reports and clinical documents.

**Key Features:**
- PDF document analysis and information extraction
- Medical terminology recognition
- Conversation-based document querying
- Structured medical information extraction
- Patient and doctor information identification

**Technical Stack:**
- LangChain for document processing pipelines
- OpenAI GPT-4 for document understanding
- FAISS for vector storage and retrieval
- PyPDF2 for PDF processing

**Server:** Not explicitly configured in code (defaults to 8000)

### Chat Microservice

A specialized medical conversational service that provides accurate medical information and guidance.

**Key Features:**
- Medical-specific conversation capabilities
- Contextual awareness with conversation history
- Streaming responses for better user experience
- Appropriate medical disclaimers and recommendations

**Technical Stack:**
- OpenAI API with GPT-4
- FastAPI with streaming response support

**Server:** Runs on port 8002

## Installation

### Prerequisites
- Python 3.9 or higher
- Node.js and npm (for frontend components)
- Ollama (for local LLM capabilities)
- Docker (optional, for containerized deployment)

### Clone the Repository
```bash
git clone https://github.com/yourusername/vitalis-ai-services.git
cd vitalis-ai-services
```

### Setup for TB-Detection API
```bash
# Navigate to the service directory
cd tb-detection-api

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Download the TB detection model
# Place modelTB.h5 in the root directory

# Start the service
python main.py
```

### Setup for AI Image Doctor
```bash
# Navigate to the service directory
cd AIImageDoctor

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Create .env file with your OpenAI API key
echo "OPENAI_API_KEY=your_api_key_here" > .env

# Start the service
python -m app.main
```

### Setup for AI Assistant Doctor
```bash
# Navigate to the service directory
cd AIAssistantDoctor

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Create .env file with your OpenAI API key
echo "OPENAI_API_KEY=your_api_key_here" > .env

# Start the service
python main.py
```

### Setup for Chat Microservice
```bash
# Navigate to the service directory
cd microservice_chat

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Create .env file with your OpenAI API key
echo "OPENAI_API_KEY=your_api_key_here" > .env

# Start the service
python main.py
```

## Configuration

Each microservice uses a `.env` file for configuration. Here are the key configuration parameters:

### TB-Detection API
No environment variables required, as it uses a local Ollama installation.

### AI Image Doctor
```
OPENAI_API_KEY=your_openai_api_key
```

### AI Assistant Doctor
```
OPENAI_API_KEY=your_openai_api_key
```

### Chat Microservice
```
OPENAI_API_KEY=your_openai_api_key
```

## API Documentation

### TB-Detection API

#### Predict Endpoint
```
POST /predict/
```
Analyzes an uploaded chest X-ray image for tuberculosis signs.

**Request:** Form data with image file
**Response:** Classification result and confidence score

#### Ollama Query Endpoint
```
POST /ollama_query/
```
Generates detailed medical context for detection results.

**Request:** JSON with class and confidence
**Response:** Detailed radiological interpretation

#### Follow-up Endpoint
```
POST /follow_up/
```
Answers follow-up questions related to the analysis.

**Request:** JSON with question and previous context
**Response:** Contextual answer

### AI Image Doctor

#### Process Endpoint
```
POST /api/v1/process
```
Analyzes a medical image with optional textual query.

**Request:** Form data with query text and optional image file
**Response:** Detailed analysis of the medical image

### AI Assistant Doctor

#### Upload Documents Endpoint
```
POST /upload-documents
```
Processes uploaded PDF medical documents for analysis.

**Request:** Form data with PDF files
**Response:** Conversation ID and extracted patient information

#### Ask Endpoint
```
POST /ask
```
Queries the processed documents.

**Request:** JSON with question and conversation ID
**Response:** Answer based on document content and chat history

#### Conversations Endpoint
```
GET /conversations
```
Retrieves all conversations.

**Response:** List of conversation metadata

#### Conversation Detail Endpoint
```
GET /conversation/{conversation_id}
```
Retrieves details of a specific conversation.

**Response:** Detailed conversation information

### Chat Microservice

#### Chat Endpoint
```
POST /api/chat
```
Provides medical conversation capabilities.

**Request:** JSON with message, history, and user_id
**Response:** Streaming text response

#### Health Check Endpoint
```
GET /health
```
Checks service status.

**Response:** Health status information

## Integration Guide

### Frontend Integration

Each microservice can be integrated with a frontend application using standard HTTP requests. Here's an example using React:

```javascript
// Example: Uploading an image to TB-Detection API
const handleImageUpload = async (file) => {
  const formData = new FormData();
  formData.append('file', file);
  
  try {
    const response = await fetch('http://localhost:8000/predict/', {
      method: 'POST',
      body: formData,
    });
    
    const result = await response.json();
    // Handle the result
    console.log(result);
  } catch (error) {
    console.error('Error:', error);
  }
};
```

### Backend Integration

For integration with existing hospital management systems, you can use their API client libraries or direct HTTP requests.

```python
# Example: Integrating with AI Assistant Doctor from another service
import requests

def process_medical_document(pdf_file):
    files = {'files': (pdf_file.filename, pdf_file.read(), 'application/pdf')}
    response = requests.post('http://localhost:8000/upload-documents', files=files)
    return response.json()
```

### Containerization

For production deployment, consider containerizing each microservice with Docker:

```dockerfile
# Example Dockerfile for TB-Detection API
FROM python:3.9

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python", "main.py"]
```

## License

This project is licensed under the MIT License - see the LICENSE file for details.

---

Created by ITNKOC © 2025

*These tools are intended to assist medical professionals and should not be used as the sole basis for diagnostic decisions.*
