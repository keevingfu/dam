# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**DAM (Digital Asset Management)** is a web-based system for managing short-form video marketing content (短视频营销DAM). It's a standalone HTML/CSS/JavaScript application targeting the Chinese market, particularly for platforms like Douyin/TikTok marketing management.

## Architecture

This is a **static web application** with a portal-based architecture:
- `portal.html` serves as the main entry point that loads other modules via iframes
- Each HTML file represents a self-contained feature module
- No build process, framework dependencies, or external libraries
- All functionality is implemented with vanilla JavaScript

## Key Modules

- **portal.html**: Main entry point with navigation to all features
- **dashboard.html**: Analytics and data overview
- **media-library.html**: Asset management and storage
- **projects.html**: Project organization
- **publishing.html**: Content publishing workflow
- **performance.html**: Performance metrics and analytics
- **video-analysis.html**: Video content analysis tools
- **script-editor.html**: Script writing and editing tools

## Development Commands

Since this is a static HTML project with no build system:
- **Run locally**: Open `portal.html` directly in a web browser or use a local server like `python -m http.server 8000`
- **No build/test/lint commands** - this is pure HTML/CSS/JS without tooling

## Code Patterns

### JavaScript Structure
- All JavaScript is inline within `<script>` tags at the bottom of each HTML file
- Event-driven architecture using `addEventListener`
- Direct DOM manipulation without frameworks
- Console.log statements indicate placeholder functionality

### CSS Organization
- Inline CSS within `<style>` tags in each file
- Consistent design system with CSS variables
- Color scheme: Blue (#1a73e8), Green (#34a853), Yellow (#fbbc04), Red (#ea4335)
- Border radius: 8px for elements, 12px for cards
- Flexbox and CSS Grid for layouts

### Common UI Components
- Cards for content display
- Modal overlays for uploads and dialogs
- Tab navigation for section switching
- Search/filter inputs with real-time filtering
- Drag-and-drop zones for file uploads

## Current State

The project is in a **prototype/demo phase**:
- UI is fully implemented with Chinese interface
- Most actions log to console or show "功能开发中..." (Feature in development) alerts
- No backend API integration yet
- Static/mock data displayed in UI
- Ready for backend integration

## Development Guidelines

When adding new features or modifying existing ones:
1. Maintain the single-file approach - each feature should be self-contained
2. Follow the existing CSS variable system for consistent theming
3. Use Chinese language for all UI text
4. Keep the same interaction patterns (console.log for actions that will connect to backend)
5. Ensure responsive design with the existing breakpoints
6. Match the existing visual design language

## Future Integration Points

Areas marked for backend integration:
- File upload handlers in media-library.html
- Data fetching in dashboard.html and performance.html
- Search functionality across all modules
- User authentication (currently no auth system)
- Project and task management in projects.html and tasks.html

## Claude AI 交互规则

### 1. 语言规则
- **交流语言**：使用中文与用户进行所有对话和交流
- **代码语言**：所有代码生成全部使用英文，包括注释、变量名、函数名等

### 2. 交互原则
- **批判性思维**：每次都用审视的目光，仔细分析用户输入的潜在问题
- **主动建议**：指出用户可能存在的问题，并给出超出用户思考框架之外的创新建议
- **建设性反馈**：以专业和尊重的方式直接指出不合理之处，并提供具体改进方案

### 3. 视频预览功能实现规范（优化版）

#### 3.1 技术架构
```javascript
// Recommended video preview component architecture
VideoPreviewCard
├── VideoThumbnail (default display)
├── VideoPlayer (lazy loaded)
├── PlatformAdapter (platform adapter)
└── LoadingState (loading state)
```

#### 3.2 平台适配策略

##### YouTube
- **可嵌入**：使用YouTube IFrame API
- **横屏视频**：16:9 标准容器
- **Shorts**：9:16 竖屏容器
```javascript
// YouTube embed example
<iframe src="https://www.youtube.com/embed/VIDEO_ID" />
```

##### TikTok
- **限制**：不支持直接嵌入播放
- **解决方案**：
  1. 显示视频封面 + TikTok logo
  2. 点击跳转到TikTok网站
  3. 或使用TikTok Embed API（仅显示，不可播放）

##### Instagram
- **限制**：严格的嵌入限制
- **解决方案**：
  1. 使用Instagram oEmbed API获取预览
  2. 显示静态预览 + 播放按钮
  3. 点击在模态框中打开

#### 3.3 视频容器标准尺寸

```css
/* Landscape video container (16:9) */
.video-container-landscape {
  aspect-ratio: 16/9;
  max-width: 100%;
  height: auto;
}

/* Portrait video container (9:16) */
.video-container-portrait {
  aspect-ratio: 9/16;
  max-height: 500px;
  width: auto;
}

/* Responsive grid layout */
.video-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 16px;
}
```

#### 3.4 性能优化策略

##### 1. 虚拟滚动实现
```javascript
// Using react-window or react-virtualized
import { FixedSizeList } from 'react-window';

<FixedSizeList
  height={600}
  itemCount={videos.length}
  itemSize={300}
  width="100%"
>
  {({ index, style }) => (
    <VideoCard video={videos[index]} style={style} />
  )}
</FixedSizeList>
```

##### 2. Intersection Observer 懒加载
```javascript
// Video lazy loading Hook
const useVideoLazyLoad = (threshold = 0.1) => {
  const [isInView, setIsInView] = useState(false);
  const ref = useRef(null);

  useEffect(() => {
    const observer = new IntersectionObserver(
      ([entry]) => setIsInView(entry.isIntersecting),
      { threshold }
    );
    if (ref.current) observer.observe(ref.current);
    return () => observer.disconnect();
  }, [threshold]);

  return [ref, isInView];
};
```

##### 3. 预加载策略
- 首屏视频：立即加载缩略图
- 第二屏视频：延迟1秒加载
- 其余视频：滚动到可视区域时加载