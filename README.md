# Smart RHP/DRHP Platform

A specialized AI-powered full-stack solution designed to simplify and accelerate the review of Red Herring Prospectus (RHP) and Draft Red Herring Prospectus (DRHP) documents. This platform enables secure document management, intelligent summarization, interactive exploration, and automated comparison between draft and final versions.

## 📋 Overview

Financial documents such as RHPs and DRHPs are often lengthy, complex, and difficult for stakeholders to analyze efficiently. The Smart RHP/DRHP Platform consolidates manual effort into an intelligent assistant, allowing investors, analysts, and corporate teams to quickly identify important details, track changes between versions, and generate comparison reports with clarity and confidence.

## 🎯 Problem Statement

Financial documents such as RHPs and DRHPs present multiple challenges for stakeholders:

### Length and Complexity
These filings often run into hundreds of pages, filled with legal, financial, and regulatory language that is difficult to parse quickly.

### Inefficient Manual Review
Teams spend days manually extracting insights, comparing drafts with final versions, and preparing executive summaries, which slows decision making.

### Difficulty in Tracking Changes
Between DRHP and RHP versions, important adjustments are often buried within dense paragraphs, making it hard to track what has changed and why.

### Limited Accessibility of Insights
Once comparison reports are prepared, they often remain static documents. Stakeholders cannot ask dynamic questions or explore content beyond the original summary.

## 💡 Solution Overview

The Smart RHP/DRHP Platform directly addresses these pain points by introducing a modern, AI-powered workspace:

- **Intelligent Summarization**: Automatic generation of concise, accurate summaries of lengthy documents so readers can focus on key insights without wading through every page.

- **Interactive Exploration**: A chat-based interface that allows users to ask questions in plain language and receive document-grounded answers instantly.

- **Automated Comparison**: Side-by-side comparison between DRHP and RHP versions, highlighting textual, numerical, and structural changes to support faster decision making.

- **Streamlined Report Generation**: On-demand creation of professional comparison reports in user-friendly formats, ready for boardrooms, client discussions, or compliance reviews.

- **Multi-Tenant Workspace Management**: Secure domain and workspace-based isolation for different clients and teams.

## 🏗️ Technical Architecture

The platform is built as a **full-stack application** with the following architecture:

### Frontend (`RHP-Document-Summarizer-main/`)
- **Framework**: React 18 + TypeScript
- **Build Tool**: Vite
- **UI Library**: Tailwind CSS + shadcn-ui components
- **State Management**: React Context + React Query
- **Routing**: React Router v6
- **HTTP Client**: Axios
- **Real-time**: Socket.io-client

### Backend (`smart-rhtp-backend-main/`)
- **Runtime**: Node.js + Express
- **Database**: MongoDB with Mongoose ODM
- **Authentication**: JWT + Passport.js (Microsoft OAuth)
- **File Storage**: Cloudflare R2 (S3-compatible)
- **Real-time**: Socket.io
- **Security**: Helmet, CORS, Rate Limiting

### AI Processing (n8n Workflow)
- **Workflow Engine**: [n8n](https://n8n.io/) - Open-source workflow automation platform
- **Vector Database**: [Pinecone](https://www.pinecone.io/) - Vector database for storing document embeddings
- **Embedding Model**: OpenAI GPT-3.5 Large - For creating document embeddings
- **Chat Model**: OpenAI GPT-4.1 Mini - For interactive chat functionality
- **Reranking**: Cohere Rerank - For ranking and improving search relevance

### Data Flow

1. **Document Upload**: Users upload PDFs through the frontend
2. **Storage**: Files are stored in Cloudflare R2 via backend API
3. **Processing**: Backend triggers n8n workflow for AI processing
4. **Embedding Generation**: n8n processes documents and creates embeddings using OpenAI GPT-3.5 Large
5. **Vector Storage**: Embeddings are stored in Pinecone vector database
6. **Interactive Chat**: Users chat with documents via frontend, backend queries n8n, which uses Pinecone + GPT-4.1 Mini
7. **Summarization**: n8n generates summaries and sends back to backend
8. **Comparison**: n8n compares DRHP and RHP versions and generates reports

## ✨ Features

### Core Features
- 📄 **Document Management**: Secure upload, storage, and organization of RHP/DRHP documents
- 🤖 **AI-Powered Summarization**: Automatic generation of accurate, comprehensive summaries
- 💬 **Interactive Chat Interface**: Ask questions and get instant, document-grounded answers using RAG
- 🔍 **Intelligent Search**: Vector-based semantic search with reranking for improved accuracy
- 📊 **Version Comparison**: Automated comparison between draft and final versions
- 📈 **Report Generation**: On-demand creation of professional comparison reports (HTML & PDF)

### Collaboration Features
- 👥 **Workspace Management**: Create and manage team workspaces with role-based access
- 🔐 **Domain Separation**: Complete data isolation between different client domains
- 📁 **Directory Organization**: Folder structure for organizing documents
- 🔗 **Document Sharing**: Share documents with specific users with granular permissions
- 📧 **Workspace Invitations**: Invite users to workspaces via email
- 🔔 **Real-time Notifications**: WebSocket-based notifications for summaries, reports, and invitations

### Security Features
- 🔒 **Multi-Factor Authentication**: Email/password + Microsoft OAuth
- 🛡️ **JWT Token Management**: Access tokens + refresh tokens with auto-refresh
- 🚦 **Rate Limiting**: Per-user and per-workspace rate limiting
- 🔐 **Data Isolation**: Domain and workspace-based data separation
- 📝 **Audit Logging**: Track all user actions and resource access

### Admin Features
- 👨‍💼 **Admin Dashboard**: User and workspace management interface
- 📊 **User Management**: View, manage, and suspend users
- 🏢 **Workspace Administration**: Manage workspaces and members
- 📈 **Activity Monitoring**: Track platform usage and activities

## 🚀 Getting Started

### Prerequisites

- **Node.js** (v18 or higher) and npm
- **MongoDB** (local or cloud instance like MongoDB Atlas)
- **Cloudflare R2** account (or AWS S3) for file storage
- **n8n instance** (self-hosted or cloud) for AI processing
- **Pinecone account** and API key
- **OpenAI API key**
- **Cohere API key** (for reranking)
- **Microsoft Azure App** (for OAuth - optional)

### Installation

1. **Clone this repository:**
```bash
git clone https://github.com/Harsh1652/DRHP-RHP.git
cd DRHP-RHP
```

2. **Set up the Backend:**

```bash
cd smart-rhtp-backend-main
npm install
```

Create a `.env` file in `smart-rhtp-backend-main/`:
```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/smart-rhp
# Or use MongoDB Atlas: mongodb+srv://user:pass@cluster.mongodb.net/smart-rhp

JWT_SECRET=your_jwt_secret_key_here
JWT_REFRESH_SECRET=your_jwt_refresh_secret_key_here

# Microsoft OAuth (optional)
CLIENT_ID=your_microsoft_client_id
CLIENT_SECRET=your_microsoft_client_secret
REDIRECT_URI=http://localhost:5000/api/auth/callback
FRONTEND_URL=http://localhost:8080

# Cloudflare R2 (S3-compatible)
R2_ACCESS_KEY_ID=your_r2_access_key_id
R2_SECRET_ACCESS_KEY=your_r2_secret_access_key
R2_BUCKET_NAME=your_r2_bucket_name
CLOUDFLARE_URI=https://<accountid>.<region>.r2.cloudflarestorage.com

# Email Service (for OTP and notifications)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASS=your_app_password
```

Start the backend:
```bash
npm run dev  # Development mode
# or
npm run build && npm start  # Production mode
```

3. **Set up the Frontend:**

```bash
cd RHP-Document-Summarizer-main
npm install
```

Create a `.env` file in `RHP-Document-Summarizer-main/`:
```env
VITE_API_URL=http://localhost:5000/api
```

Start the frontend:
```bash
npm run dev
```

The frontend will be available at `http://localhost:8080`

4. **Set up n8n Workflow:**

- Deploy or access your n8n instance
- Import the `N8n.json` workflow file from the root directory
- Configure the required credentials in n8n:
  - OpenAI API credentials
  - Pinecone API credentials
  - Cohere API credentials
- Update the webhook URLs in the workflow to point to your backend
- Activate the workflow

5. **Run Database Migrations (if needed):**

```bash
cd smart-rhtp-backend-main
npm run migrate:domain  # For domain separation
npm run migrate:workspace  # For workspace support
```

## 📖 Usage

### User Registration & Authentication

1. **Register**: Create an account with email/password or use Microsoft OAuth
2. **Login**: Access the platform with your credentials
3. **Workspace**: Select or create a workspace to organize your documents

### Document Management

1. **Upload Documents**: 
   - Navigate to Dashboard
   - Click "Upload Document"
   - Select DRHP or RHP type
   - Upload PDF file
   - System automatically processes and creates embeddings

2. **Organize Documents**:
   - Create directories/folders
   - Move documents between directories
   - Rename and manage documents

### AI Features

1. **Chat with Documents**:
   - Open a document
   - Use the chat interface
   - Ask questions in plain language
   - Receive instant, document-grounded answers

2. **Generate Summaries**:
   - Open a document
   - Click "Generate Summary"
   - Wait for AI processing (notification when ready)
   - View comprehensive summary

3. **Compare Versions**:
   - Upload both DRHP and RHP versions
   - Navigate to Compare page
   - Select both documents
   - Generate comparison report
   - Download PDF report when ready

### Workspace Collaboration

1. **Create Workspace**: Admin can create new workspaces
2. **Invite Members**: Send invitations via email
3. **Manage Permissions**: Set user roles (admin, user, viewer, editor)
4. **Share Documents**: Share specific documents with team members

## 🔧 Configuration

### Environment Variables

#### Backend Environment Variables

See the `.env` example in the Installation section above.

#### Frontend Environment Variables

- `VITE_API_URL`: Backend API URL (default: `http://localhost:5000/api`)

#### n8n Workflow Configuration

Ensure the following are configured in your n8n workflow:
- `OPENAI_API_KEY`: Your OpenAI API key
- `PINECONE_API_KEY`: Your Pinecone API key
- `PINECONE_ENVIRONMENT`: Your Pinecone environment
- `COHERE_API_KEY`: Your Cohere API key

### Database Setup

1. **MongoDB**: Create a database and update `MONGODB_URI` in backend `.env`
2. **Indexes**: The application automatically creates necessary indexes on first run
3. **Migrations**: Run migration scripts if upgrading from older versions

### File Storage Setup

1. **Cloudflare R2**:
   - Create an R2 bucket
   - Generate access keys
   - Update backend `.env` with R2 credentials

2. **Alternative**: Can use AWS S3 by updating the S3 client configuration

### Pinecone Index Configuration

- Create a Pinecone index with appropriate dimensions for your embedding model (typically 1536 for OpenAI embeddings)
- Configure the index name in the n8n workflow nodes

## 📁 Project Structure

```
DRHP-RHP/
├── RHP-Document-Summarizer-main/    # Frontend React application
│   ├── src/
│   │   ├── components/              # React components
│   │   ├── pages/                   # Page components
│   │   ├── services/                # API services
│   │   ├── contexts/                # React contexts
│   │   ├── hooks/                   # Custom hooks
│   │   └── lib/                     # Utilities and API clients
│   ├── public/                      # Static assets
│   ├── package.json
│   └── vite.config.ts
│
├── smart-rhtp-backend-main/          # Backend Express API
│   ├── controllers/                  # Request handlers
│   ├── models/                       # Mongoose schemas
│   ├── routes/                       # API routes
│   ├── middleware/                   # Express middleware
│   ├── services/                     # Business logic services
│   ├── config/                       # Configuration files
│   ├── scripts/                      # Migration scripts
│   ├── lib/                          # Utilities
│   ├── index.ts                      # Server entry point
│   └── package.json
│
├── N8n.json                          # n8n workflow configuration
├── README.md                         # This file
└── PROJECT_EXPLANATION.md            # Detailed file-by-file explanation
```

For a comprehensive explanation of every file in the project, see [PROJECT_EXPLANATION.md](./PROJECT_EXPLANATION.md).

## 🧪 Development

### Running in Development Mode

**Backend:**
```bash
cd smart-rhtp-backend-main
npm run dev  # Uses nodemon for auto-reload
```

**Frontend:**
```bash
cd RHP-Document-Summarizer-main
npm run dev  # Vite dev server on port 8080
```

### Building for Production

**Backend:**
```bash
cd smart-rhtp-backend-main
npm run build  # Compiles TypeScript to dist/
npm start      # Runs production server
```

**Frontend:**
```bash
cd RHP-Document-Summarizer-main
npm run build  # Builds to dist/
npm run preview  # Preview production build
```

## 🚢 Deployment

### Frontend Deployment (Vercel)

1. Connect your repository to Vercel
2. Set build command: `npm run build`
3. Set output directory: `dist`
4. Add environment variable: `VITE_API_URL`

### Backend Deployment (Render/Heroku)

1. Connect your repository
2. Set build command: `npm run build`
3. Set start command: `npm start`
4. Add all environment variables from `.env`
5. Ensure MongoDB connection is accessible

### n8n Deployment

- Deploy n8n instance (self-hosted or cloud)
- Import `N8n.json` workflow
- Configure credentials
- Update webhook URLs to point to deployed backend

## 🔒 Security Considerations

- **JWT Tokens**: Short-lived access tokens with long-lived refresh tokens
- **Password Hashing**: bcryptjs with salt rounds
- **CORS**: Configured for specific origins
- **Rate Limiting**: 500 requests/minute globally, per-user and per-workspace limits
- **Helmet**: Security headers middleware
- **Input Validation**: Zod schema validation
- **Domain Isolation**: Complete data separation between client domains
- **Workspace Permissions**: Role-based access control (RBAC)

## 📚 API Documentation

### Authentication Endpoints

- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login with email/password
- `POST /api/auth/refresh-token` - Refresh JWT token
- `POST /api/auth/logout` - Logout
- `GET /api/auth/microsoft` - Microsoft OAuth URL
- `GET /api/auth/callback` - OAuth callback
- `GET /api/auth/me` - Get current user

### Document Endpoints

- `GET /api/documents` - List documents
- `POST /api/documents/upload` - Upload document
- `GET /api/documents/:id` - Get document
- `GET /api/documents/download/:id` - Download document
- `PUT /api/documents/:id` - Update document
- `DELETE /api/documents/:id` - Delete document

### Chat Endpoints

- `GET /api/chats` - List chats
- `POST /api/chats` - Create chat
- `POST /api/chats/:chatId/messages` - Send message
- `GET /api/chats/document/:documentId` - Get document chat

### Summary Endpoints

- `GET /api/summaries` - List summaries
- `POST /api/summaries` - Generate summary
- `GET /api/summaries/document/:documentId` - Get document summary

### Report Endpoints

- `GET /api/reports` - List reports
- `POST /api/reports` - Generate comparison report
- `GET /api/reports/:id` - Get report
- `GET /api/reports/download/:id` - Download report PDF

For complete API documentation, see the [Backend README](./smart-rhtp-backend-main/README.md).

## 🐛 Troubleshooting

### Common Issues

1. **MongoDB Connection Error**:
   - Verify `MONGODB_URI` is correct
   - Check MongoDB is running (if local)
   - Verify network access (if cloud)

2. **File Upload Fails**:
   - Check Cloudflare R2 credentials
   - Verify bucket name and permissions
   - Check file size limits

3. **Authentication Issues**:
   - Verify JWT secrets are set
   - Check token expiration settings
   - Clear browser localStorage and retry

4. **n8n Workflow Not Triggering**:
   - Verify webhook URLs in workflow
   - Check n8n instance is accessible
   - Verify API credentials in n8n

5. **Frontend Can't Connect to Backend**:
   - Check `VITE_API_URL` environment variable
   - Verify backend is running
   - Check CORS settings in backend

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### Contribution Guidelines

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Follow TypeScript best practices
- Write meaningful commit messages
- Add comments for complex logic
- Update documentation for new features
- Test your changes thoroughly

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## 👥 Authors

- **Harsh1652** - [GitHub Profile](https://github.com/Harsh1652)

## 🙏 Acknowledgments

- **n8n** - Workflow automation platform
- **OpenAI** - Embedding and chat models (GPT-3.5 Large, GPT-4.1 Mini)
- **Pinecone** - Vector database infrastructure
- **Cohere** - Reranking capabilities
- **React Team** - React framework
- **Vercel** - Frontend hosting
- **Render** - Backend hosting
- **Cloudflare** - R2 object storage
- **shadcn** - UI component library

## 📞 Support

For issues, questions, or contributions:
- Open an issue on the [GitHub repository](https://github.com/Harsh1652/DRHP-RHP)
- Check the [PROJECT_EXPLANATION.md](./PROJECT_EXPLANATION.md) for detailed documentation
- Review the [Backend README](./smart-rhtp-backend-main/README.md) for API details
- Review the [Frontend README](./RHP-Document-Summarizer-main/README.md) for frontend details

## 📖 Additional Documentation

- **[PROJECT_EXPLANATION.md](./PROJECT_EXPLANATION.md)**: Comprehensive file-by-file explanation of the entire codebase
- **[DOMAIN_SEPARATION_GUIDE.md](./smart-rhtp-backend-main/DOMAIN_SEPARATION_GUIDE.md)**: Guide for domain/workspace separation implementation
- **[Backend README](./smart-rhtp-backend-main/README.md)**: Backend API documentation
- **[Frontend README](./RHP-Document-Summarizer-main/README.md)**: Frontend setup and usage

## 🔄 Version History

- **v1.0.0** - Initial release with core features
  - Document upload and management
  - AI-powered summarization
  - Interactive chat interface
  - Version comparison
  - Workspace management
  - Domain separation

---

**Note**: This platform is designed for financial document analysis and should be used in compliance with relevant regulations and data privacy requirements. Ensure proper data handling and security measures are in place for production deployments.
