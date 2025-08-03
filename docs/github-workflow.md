# 📂 GitHub 워크플로우 가이드

## 🚀 **미디어 제작 자동화를 위한 GitHub 기반 워크플로우**

이 문서는 MCP Claude가 GitHub API를 활용하여 미디어 제작 프로젝트를 자동으로 관리하는 방법을 설명합니다.

---

## 📋 **프로젝트 라이프사이클**

### 1️⃣ **프로젝트 시작**
```python
# 새 프로젝트 브랜치 생성
github:create_branch(
    repo="media-automation-ai-assistant", 
    branch="project/AI-자동화-20250803"
)
```

### 2️⃣ **프로젝트 폴더 구조 생성**
```python
# 프로젝트 초기 구조 생성
github:push_files(
    branch="project/AI-자동화-20250803",
    files=[
        {
            "path": "projects/AI-자동화-20250803/README.md",
            "content": "# AI 자동화 프로젝트\n\n프로젝트 시작일: 2025-08-03"
        },
        {
            "path": "projects/AI-자동화-20250803/project_log.md", 
            "content": "# 프로젝트 진행 로그\n\n## 2025-08-03\n- 프로젝트 시작"
        }
    ],
    message="🚀 AI 자동화 프로젝트 초기 구조 생성"
)
```

### 3️⃣ **단계별 파일 저장**
```python
# 대본 저장
github:create_or_update_file(
    path="projects/AI-자동화-20250803/script.md",
    content="# 대본 내용...",
    message="📝 대본 작성 완료",
    branch="project/AI-자동화-20250803"
)

# 음성 파일 저장 (Base64 인코딩)
github:create_or_update_file(
    path="projects/AI-자동화-20250803/voice.mp3",
    content="[Base64 인코딩된 음성 데이터]",
    message="🎙️ 음성 합성 완료",
    branch="project/AI-자동화-20250803"
)

# 이미지 파일들 저장
github:push_files(
    branch="project/AI-자동화-20250803",
    files=[
        {"path": "projects/AI-자동화-20250803/images/scene_01.png", "content": "[이미지 데이터]"},
        {"path": "projects/AI-자동화-20250803/images/scene_02.png", "content": "[이미지 데이터]"},
        {"path": "projects/AI-자동화-20250803/images/scene_03.png", "content": "[이미지 데이터]"}
    ],
    message="🖼️ 장면별 이미지 생성 완료"
)
```

### 4️⃣ **프로젝트 완료 및 병합**
```python
# Pull Request 생성
github:create_pull_request(
    head="project/AI-자동화-20250803",
    base="main",
    title="🎬 AI 자동화 숏폼 영상 제작 프로젝트 완료",
    body="""
## 🎯 프로젝트 요약
- **주제**: AI 자동화 기술의 현재와 미래
- **대상**: 일반인 대상 숏폼 영상
- **길이**: 1분 내외

## 📊 생성된 미디어
- ✅ 대본 (script.md)
- ✅ 음성 파일 (voice.mp3)
- ✅ 배경음악 (bgm.mp3)
- ✅ 장면별 이미지 (images/)
- ✅ 분석 결과 (analysis.md)

## 🎬 편집 설정
- 해상도: 1080x1920 (세로)
- 자막 크기: 24pt
- 템포: 1.2배속
- 플랫폼: 유튜브 숏츠, 인스타그램 릴스
    """
)

# PR 자동 병합 (옵션)
github:merge_pull_request(
    pullNumber=[PR번호],
    merge_method="squash"
)
```

---

## 🔄 **지속적인 프로젝트 관리**

### **프로젝트 상태 확인**
```python
# 기존 프로젝트 목록 확인
github:get_file_contents(path="projects/")

# 특정 프로젝트 로그 확인
github:get_file_contents(path="projects/AI-자동화-20250803/project_log.md")
```

### **프로젝트 업데이트**
```python
# 대본 수정
github:create_or_update_file(
    path="projects/AI-자동화-20250803/script.md",
    content="수정된 대본 내용",
    message="📝 대본 수정 - 도입부 강화",
    sha="[기존 파일의 SHA]"
)
```

### **버전 히스토리 추적**
```python
# 커밋 히스토리 확인
github:list_commits(
    repo="media-automation-ai-assistant",
    sha="project/AI-자동화-20250803"
)
```

---

## 📂 **표준 폴더 구조**

```
projects/[프로젝트명-YYYYMMDD]/
├── 📝 README.md (프로젝트 개요)
├── 📋 script.md (최종 대본)
├── 📋 script_outline.md (대본 구조)
├── 📊 research_summary.md (조사 결과)
├── 🎙️ voice.mp3 (음성 파일)
├── 🎵 bgm.mp3 (배경음악)
├── 📁 images/
│   ├── scene_01.png
│   ├── scene_02.png
│   └── scene_03.png
├── 🎬 video_config.json (편집 설정)
├── 📊 analysis.md (whisper 분석)
├── 📋 project_summary.md (프로젝트 요약)
└── 📄 project_log.md (진행 로그)
```

---

## 🎯 **자동화 스크립트 예시**

### **완전 자동화 워크플로우**
```python
def create_media_project(topic, date):
    project_name = f"{topic}-{date}"
    branch_name = f"project/{project_name}"
    
    # 1. 브랜치 생성
    github:create_branch(repo="media-automation-ai-assistant", branch=branch_name)
    
    # 2. 조사 및 대본 작성
    research_result = web_search(topic)
    script_content = create_script(research_result)
    
    # 3. 음성 합성
    voice_file = elevenlabs_tts(script_content)
    
    # 4. 이미지 생성
    images = []
    for scene in script_scenes:
        image = openai_image_generate(scene.description)
        images.append(image)
    
    # 5. GitHub에 모든 파일 저장
    github:push_files(
        branch=branch_name,
        files=[
            {"path": f"projects/{project_name}/script.md", "content": script_content},
            {"path": f"projects/{project_name}/voice.mp3", "content": voice_file},
            *[{"path": f"projects/{project_name}/images/scene_{i}.png", "content": img} 
              for i, img in enumerate(images, 1)]
        ],
        message="🎬 미디어 프로젝트 자동 생성 완료"
    )
    
    # 6. Pull Request 생성
    github:create_pull_request(
        head=branch_name,
        base="main",
        title=f"🎬 {topic} 숏폼 영상 제작 완료",
        body=f"자동화된 {topic} 프로젝트 완료"
    )
```

---

## 📊 **프로젝트 메트릭스 추적**

### **성과 지표**
```json
{
  "project_metrics": {
    "total_projects": 15,
    "completed_projects": 12,
    "average_completion_time": "2.5 hours",
    "success_rate": "80%",
    "popular_topics": ["AI", "테크", "라이프스타일"],
    "platform_optimization": {
      "youtube_shorts": 8,
      "instagram_reels": 6,
      "tiktok": 4
    }
  }
}
```

### **품질 지표**
```json
{
  "quality_metrics": {
    "script_accuracy": "95%",
    "voice_quality": "4.8/5.0",
    "image_relevance": "4.6/5.0",
    "editing_optimization": "4.7/5.0",
    "user_satisfaction": "4.5/5.0"
  }
}
```

---

## 🔐 **보안 및 베스트 프랙티스**

### **파일 보안**
- **민감 데이터**: 개인정보나 저작권이 있는 콘텐츠는 private repository 사용
- **Access Token**: GitHub API 토큰은 환경 변수로 관리
- **Large Files**: 100MB 이상 파일은 Git LFS 사용

### **브랜치 관리**
- **Naming Convention**: `project/[주제명-YYYYMMDD]`
- **Branch Protection**: main 브랜치는 direct push 금지
- **Code Review**: 모든 PR은 리뷰 후 병합

### **커밋 메시지 컨벤션**
```
🚀 프로젝트 시작
📊 조사 결과 업데이트  
📝 대본 작성 완료
🎙️ 음성 합성 완료
🎵 배경음악 추가
🖼️ 이미지 생성 완료
🎬 편집 설정 완료
📋 프로젝트 요약 완료
🔄 파일 수정
✅ 프로젝트 완료
```

---

## 🌐 **GitHub Pages 배포**

### **자동 배포 설정**
```yaml
# .github/workflows/deploy.yml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Setup Pages
        uses: actions/configure-pages@v2
      - name: Build site
        run: |
          # 프로젝트 갤러리 생성 스크립트 실행
          python scripts/generate_gallery.py
      - name: Deploy to Pages
        uses: actions/deploy-pages@v1
```

### **프로젝트 갤러리 생성**
```python
# scripts/generate_gallery.py
import os
import json
from pathlib import Path

def generate_project_gallery():
    projects = []
    projects_dir = Path("projects")
    
    for project_dir in projects_dir.iterdir():
        if project_dir.is_dir():
            summary_file = project_dir / "project_summary.md"
            if summary_file.exists():
                project_info = parse_project_summary(summary_file)
                projects.append(project_info)
    
    # HTML 갤러리 생성
    generate_html_gallery(projects)

if __name__ == "__main__":
    generate_project_gallery()
```

---

## 📈 **분석 및 리포팅**

### **월간 리포트 자동 생성**
```python
def generate_monthly_report():
    # 지난 30일간 프로젝트 분석
    projects = get_recent_projects(days=30)
    
    report = {
        "period": "2025-07",
        "total_projects": len(projects),
        "completion_rate": calculate_completion_rate(projects),
        "popular_topics": analyze_popular_topics(projects),
        "quality_trends": analyze_quality_trends(projects),
        "recommendations": generate_recommendations(projects)
    }
    
    # GitHub에 리포트 저장
    github:create_or_update_file(
        path="reports/2025-07-monthly-report.md",
        content=format_report_markdown(report),
        message="📊 2025년 7월 월간 리포트 생성"
    )
```

### **대시보드 URL**
- **프로젝트 갤러리**: https://justdoit24419.github.io/media-automation-ai-assistant/
- **월간 리포트**: https://justdoit24419.github.io/media-automation-ai-assistant/reports/
- **API 통계**: https://justdoit24419.github.io/media-automation-ai-assistant/stats/

---

## 🔧 **트러블슈팅**

### **자주 발생하는 문제**

#### 1. **파일 크기 제한**
```python
# 100MB 이상 파일 처리
if file_size > 100_000_000:  # 100MB
    # Git LFS 사용 또는 외부 스토리지 연동
    upload_to_external_storage(file)
else:
    github:create_or_update_file(path, content)
```

#### 2. **API Rate Limit**
```python
import time

def safe_github_operation(operation, *args, **kwargs):
    try:
        return operation(*args, **kwargs)
    except RateLimitError:
        time.sleep(60)  # 1분 대기 후 재시도
        return operation(*args, **kwargs)
```

#### 3. **브랜치 충돌**
```python
# 브랜치 이름 중복 체크
existing_branches = github:list_branches(repo="media-automation-ai-assistant")
if branch_name in existing_branches:
    branch_name = f"{branch_name}-v2"
```

---

## 📚 **추가 리소스**

- **GitHub API 문서**: https://docs.github.com/en/rest
- **Git LFS 가이드**: https://git-lfs.github.io/
- **GitHub Actions**: https://docs.github.com/en/actions
- **GitHub Pages**: https://docs.github.com/en/pages

**Repository**: https://github.com/Justdoit24419/media-automation-ai-assistant