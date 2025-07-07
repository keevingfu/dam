# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**DAM (Digital Asset Management)** is a web-based system for managing short-form video marketing content (短视频营销DAM). It's a standalone HTML/CSS/JavaScript application for Eureka Robot Vacuum marketing, targeting multiple platforms (TikTok, Instagram, YouTube, Facebook) with an English-language interface.

## Architecture

This is a **static web application** with a portal-based architecture:
- `index.html` serves as the main entry point (NOT portal.html as mentioned in README)
- Each HTML file represents a self-contained feature module loaded via iframes
- No build process, framework dependencies, or external libraries
- All functionality is implemented with vanilla JavaScript

## Key Modules

### Workspace
- **dashboard.html**: Analytics and data overview with tabs (Best Sellers, Top Rated, Demo Views, New Models)
- **projects.html**: Project management with tabs (All Projects, Active, Pending Review, Archived)
- **tasks.html**: Task management center with tabs (All Tasks, To Do, In Progress, Review, Completed)

### Asset Management
- **media-library.html**: Central asset storage with tabs (All Media, Product Demos, Feature Highlights, Customer Reviews, Installation Guides, Favorites)
- **competitor-collection.html**: Competitor content analysis with tabs (Latest Collection, Trending Content, Marketing Strategies, Pricing Analysis)
- **highlights.html**: Key moments library with tabs (Product Demos, Before & After, Customer Reviews, Call to Action, All Clips)
- **smart-folders.html**: Rule-based automatic organization

### Creative Collaboration
- **video-analysis.html**: Collaborative video analysis
- **script-editor.html**: Script writing with templates (Product Demo, Feature Highlight, Comparison, How-To Guide, Lifestyle, Blank) and asset tabs (Media Library, Key Moments, References)
- **review.html**: Online review and approval
- **publishing.html**: Multi-platform publishing with tabs (All Posts, Scheduled, Publishing, Published, Failed)

### Data Analysis
- **performance.html**: Performance metrics with ECharts visualizations
- **scoring.html**: Content quality scoring system
- **reuse-analysis.html**: Asset reuse tracking

## Development Commands

Since this is a static HTML project with no build system:
- **Run locally**: Open `index.html` directly in a web browser or use a local server like `python -m http.server 8000`
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
- Color scheme: Blue (#1a73e8), Green (#34a853), Yellow (#fbbc04), Red (#ea4335), Purple (#9333ea), Teal (#10b981)
- Border radius: 8px for elements, 12px for cards, 16px for major sections
- Flexbox and CSS Grid for layouts
- Responsive breakpoint at 768px

### Common UI Components
- Cards for content display
- Modal overlays for uploads and dialogs
- Tab navigation for section switching
- Search/filter inputs with real-time filtering
- Drag-and-drop zones for file uploads
- Platform-specific logos (SVG icons for TikTok, Instagram, YouTube, Facebook)

## Navigation System

The application uses a sidebar navigation with collapsible sections:
1. Click on main menu items to expand/collapse submenus
2. Submenus load corresponding HTML files in an iframe
3. Active states are maintained visually
4. Quick action cards on homepage provide direct access to key features

## Current State

The project is in a **functional prototype phase**:
- UI is fully implemented with English interface for Eureka Robot Vacuum marketing
- Tab-based navigation implemented across all major modules
- Interactive features including drag-and-drop, modal dialogs, and real-time search
- ECharts integration for data visualization (dashboard and performance modules)
- Most actions log to console or show "Feature in development..." alerts
- No backend API integration yet
- Static/mock data displayed in UI with Eureka product information
- Ready for backend integration

## Development Guidelines

When adding new features or modifying existing ones:
1. Maintain the single-file approach - each feature should be self-contained
2. Follow the existing CSS variable system for consistent theming
3. Use English language for all UI text (product is for Eureka Robot Vacuum)
4. Keep the same interaction patterns (console.log for actions that will connect to backend)
5. Ensure responsive design with the existing breakpoints
6. Match the existing visual design language
7. Add new modules to the navigation menu in portal.html following the existing pattern
8. Implement tab-based navigation with data-tab attributes for multi-section pages
9. Use consistent color coding for module types (blue for workspace, purple for assets, etc.)

## Future Integration Points

Areas marked for backend integration:
- File upload handlers in media-library.html
- Data fetching in dashboard.html and performance.html
- Search functionality across all modules
- User authentication (currently no auth system)
- Project and task management in projects.html and tasks.html
- Platform API integrations for publishing.html
- Real-time collaboration features in video-analysis.html

## Claude AI 交互规则

### 1. 语言规则
- **交流语言**：使用中文与用户进行所有对话和交流
- **UI语言**：所有界面文本使用英文（针对Eureka Robot Vacuum国际市场）
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

## Recent Updates

### Phase 1: Initial Setup (Completed)
- Translated all UI from Chinese to English
- Customized for Eureka Robot Vacuum marketing
- Implemented portal-based navigation system

### Phase 2: Data Visualization (Completed)
- Added ECharts to dashboard.html for sales trends and channel distribution
- Added ECharts to performance.html for engagement metrics and geographic data

### Phase 3: Tab Implementation (Completed)
- **tasks.html**: 5 tabs (All Tasks, To Do, In Progress, Review, Completed)
- **media-library.html**: 6 tabs (All Media, Product Demos, Feature Highlights, Customer Reviews, Installation Guides, Favorites)
- **competitor-collection.html**: 4 tabs (Latest Collection, Trending Content, Marketing Strategies, Pricing Analysis)
- **highlights.html**: 5 tabs (Product Demos, Before & After, Customer Reviews, Call to Action, All Clips)
- **publishing.html**: 5 tabs (All Posts, Scheduled, Publishing, Published, Failed)
- **script-editor.html**: 3 asset panel tabs (Media Library, Key Moments, References)
- **dashboard.html**: 4 tabs (Best Sellers, Top Rated, Demo Views, New Models)

### Phase 4: Script Templates (Completed)
- Implemented 6 script templates in script-editor.html
- Dynamic content loading based on template selection
- Pre-populated scenes with Eureka-specific content