# VideoResizer - Online Video Resizing Tool

A modern, responsive web application that replicates the Videobolt video resizing tool with all its key features and design elements. Built with React, TypeScript, and FFmpeg.wasm for client-side video processing.

## 🚀 Features

### Core Features
- **Video Resizing**: Change video dimensions while maintaining quality
- **Multiple Format Support**: MP4, MOV, AVI, WebM input/output
- **Social Media Presets**: Instagram, YouTube, TikTok, Facebook, Twitter
- **Quality Control**: High, Medium, Low quality options
- **Client-side Processing**: All processing happens in your browser
- **Real-time Preview**: See changes before processing

### Advanced Features
- **Custom Dimensions**: Set any width and height
- **Aspect Ratio Locking**: Maintain original proportions
- **Drag & Drop Upload**: Easy file selection
- **Progress Tracking**: Real-time processing feedback
- **Responsive Design**: Works on all devices

## 🛠️ Tech Stack

- **Frontend**: React 18 + TypeScript
- **Styling**: Tailwind CSS
- **Video Processing**: FFmpeg.wasm
- **Animations**: Framer Motion
- **State Management**: React Query
- **Icons**: Lucide React
- **Build Tool**: Vite

## 📦 Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/Vaibhavpandey007/video-resizer.git
   cd video-resizer
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Start development server**
   ```bash
   npm run dev
   ```

4. **Open your browser**
   Navigate to `http://localhost:5173`

## 🏗️ Project Structure

```
src/
├── components/
│   ├── ui/           # Reusable UI components
│   ├── video/        # Video-specific components
│   └── layout/       # Layout components
├── pages/            # Page components
├── types/            # TypeScript type definitions
├── utils/            # Utility functions
├── constants/        # App constants
└── main.tsx         # App entry point
```

## 🎨 Design System

### Colors
- **Primary**: Modern blue/purple gradient (#6366f1 to #8b5cf6)
- **Secondary**: Clean whites and light grays
- **Accent**: Bright green for success states (#10b981)
- **Text**: Dark grays for readability (#374151)

### Typography
- **Font**: Inter (Google Fonts)
- **Weights**: 300, 400, 500, 600, 700, 800

## 📱 Responsive Design

- **Mobile-first approach**
- **Tablet optimization**
- **Desktop enhancements**
- **Touch-friendly controls**

## 🔧 Configuration

### Environment Variables
Create a `.env` file in the root directory:

```env
VITE_APP_NAME=VideoResizer
VITE_APP_VERSION=1.0.0
```

### FFmpeg Configuration
The app uses FFmpeg.wasm for client-side video processing. The configuration is handled in `src/utils/videoProcessing.ts`.

## 🚀 Deployment

### Build for Production
```bash
npm run build
```

### Deploy to Vercel
```bash
npm install -g vercel
vercel
```

### Deploy to Netlify
```bash
npm run build
# Upload dist/ folder to Netlify
```

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📞 Support

- **Email**: vaibhavpanday245@gmail.com
- **Documentation**: [docs.videoresizer.com](https://docs.videoresizer.com)
- **Issues**: [GitHub Issues](https://github.com/Vaibhavpandey007/video-resizer/issues)

## 🙏 Acknowledgments

- [FFmpeg.wasm](https://github.com/ffmpegwasm/ffmpeg.wasm) for client-side video processing
- [Tailwind CSS](https://tailwindcss.com) for styling
- [Framer Motion](https://www.framer.com/motion/) for animations
- [Lucide React](https://lucide.dev) for icons

## 📈 Roadmap

- [ ] Batch processing
- [ ] Video trimming
- [ ] Video cropping
- [ ] Speed control
- [ ] Audio extraction
- [ ] Cloud storage integration
- [ ] API for developers
- [ ] Mobile app

---

Made with ❤️ 

---

### 1. **FFmpeg.wasm Initialization Issue**
- FFmpeg.wasm ko load hone mein time lag sakta hai, ya kabhi kabhi woh sahi tarike se load nahi hota.
- Agar aapke console mein koi error aa raha hai (jaise: "Failed to load ffmpeg-core.wasm" ya "WebAssembly not supported"), toh woh bhi reason ho sakta hai.

### 2. **Progress Callback Not Working**
- FFmpeg.wasm ka progress callback sahi implement nahi hua toh UI stuck lag sakta hai.

### 3. **Large File/Browser Memory**
- Agar file bahut badi hai, toh browser memory ya processing slow ho sakta hai.

---

## **Quick Fixes & Debug Steps**

### **A. Console Errors Check Karein**
1. Browser console (F12) open karein aur dekhein koi error aa raha hai kya? (jaise: CORS, wasm load, memory, etc.)

### **B. FFmpeg.wasm Load Code Improve Karein**
Aapke `src/utils/videoProcessing.ts` me initialization code ko thoda robust banaate hain:

#### **1. FFmpeg.wasm ko ek hi baar load karein, aur loading state handle karein:**

```ts
import { FFmpeg } from '@ffmpeg/ffmpeg';
import { fetchFile, toBlobURL } from '@ffmpeg/util';

let ffmpeg: FFmpeg | null = null;
let isFFmpegLoading = false;

export const initializeFFmpeg = async (): Promise<FFmpeg> => {
  if (ffmpeg) return ffmpeg;
  if (isFFmpegLoading) {
    // Wait until ffmpeg is loaded
    return new Promise((resolve) => {
      const check = () => {
        if (ffmpeg) resolve(ffmpeg);
        else setTimeout(check, 100);
      };
      check();
    });
  }
  isFFmpegLoading = true;
  ffmpeg = new FFmpeg();
  const baseURL = 'https://unpkg.com/@ffmpeg/core@0.12.0/dist/umd';
  await ffmpeg.load({
    coreURL: await toBlobURL(`${baseURL}/ffmpeg-core.js`, 'text/javascript'),
    wasmURL: await toBlobURL(`${baseURL}/ffmpeg-core.wasm`, 'application/wasm'),
  });
  isFFmpegLoading = false;
  return ffmpeg;
};
```

#### **2. Progress Callback ko Ignore na karein**
Aapke `resizeVideo` function me `onProgress` ko use karein, ya at least UI ko update karte rahein.

#### **3. Error Handling Add Karein**
Agar FFmpeg load ya process nahi ho raha, toh user ko error dikhaayein.

---

## **C. Demo ke liye Small Video File Try Karein**
- Pehle ek chhota (2-3 MB) video file se test karein.

---

## **D. Network Issues**
- Kabhi kabhi CDN se ffmpeg-core.wasm load nahi hota, toh aap local copy bhi rakh sakte hain.

---

## **Next Steps:**
1. Kya aapko browser console me koi error dikh raha hai? (screenshot ya error paste karein)
2. Agar nahi, toh main aapke `videoProcessing.ts` ko update kar deta hoon taaki FFmpeg.wasm loading robust ho jaaye.

Aap batao, kya aapko koi error dikh raha hai ya main direct code fix kar doon?  
Agar code fix chahiye toh "haan" bol do, main turant update kar deta hoon! 