# 📁 프로젝트 템플릿

## 🚀 **빠른 시작 가이드**

### **1단계: 프로젝트 초기화**
```bash
# Desktop Commander로 프로젝트 폴더 생성
mkdir -p "/Users/jaehwajeong/Desktop/[프로젝트명]"
cd "/Users/jaehwajeong/Desktop/[프로젝트명]"

# 기본 구조 생성
mkdir -p {scripts,audio,images,analysis,docs,backup}
```

### **2단계: 상태 파일 초기화**
```json
{
  "project_name": "[프로젝트명]",
  "created_date": "2025-08-03",
  "current_step": 1,
  "completed_steps": [],
  "total_steps": 8,
  "status": "initialized"
}
```

## 📂 **표준 프로젝트 구조**

```
[프로젝트명]/
├── 📝 scripts/
│   ├── script.txt              # 최종 대본
│   ├── script_outline.md       # 대본 구조
│   └── research_summary.md     # 조사 내용
├── 🎙️ audio/
│   ├── voice.mp3              # 메인 음성
│   ├── bgm.mp3                # 배경음악
│   └── mixed_audio.mp3        # 최종 믹싱
├── 🖼️ images/
│   ├── scene_01.png           # 장면별 이미지
│   ├── scene_02.png
│   └── thumbnails/            # 썸네일 후보들
├── 📊 analysis/
│   ├── whisper_output.txt     # 음성 인식 결과
│   ├── analysis.txt           # 품질 분석
│   └── accuracy_report.md     # 정확도 리포트
├── 📋 docs/
│   ├── project_summary.md     # 프로젝트 요약
│   ├── file_list.txt          # 파일 목록
│   └── usage_guide.md         # 활용 가이드
├── 🔄 backup/
│   ├── step_1_20250803_1030/  # 단계별 백업
│   ├── step_2_20250803_1045/
│   └── auto_save/             # 자동 저장
├── 🎬 output/
│   ├── youtube_shorts.mp4     # 유튜브용
│   ├── instagram_reels.mp4    # 인스타그램용
│   ├── tiktok.mp4             # 틱톡용
│   └── master.mov             # 마스터 파일
├── 📊 project_status.json     # 진행 상황
└── 📄 project_log.txt         # 상세 로그
```

## 🛠️ **자동화 스크립트 템플릿**

### **프로젝트 초기화 스크립트**
```python
# project_init.py
import os
import json
from datetime import datetime

def create_project_structure(project_name):
    base_path = f"/Users/jaehwajeong/Desktop/{project_name}"
    
    folders = [
        "scripts", "audio", "images", "analysis", 
        "docs", "backup", "output", "images/thumbnails", 
        "backup/auto_save"
    ]
    
    for folder in folders:
        os.makedirs(f"{base_path}/{folder}", exist_ok=True)
    
    # 상태 파일 생성
    status = {
        "project_name": project_name,
        "created_date": datetime.now().strftime("%Y-%m-%d"),
        "current_step": 1,
        "completed_steps": [],
        "total_steps": 8,
        "status": "initialized"
    }
    
    with open(f"{base_path}/project_status.json", "w") as f:
        json.dump(status, f, indent=2, ensure_ascii=False)
    
    print(f"✅ 프로젝트 '{project_name}' 초기화 완료!")
```

### **진행 상황 추적 스크립트**
```python
# progress_tracker.py
def update_progress(project_path, step_number, description):
    import json
    from datetime import datetime
    
    status_file = f"{project_path}/project_status.json"
    
    with open(status_file, "r") as f:
        status = json.load(f)
    
    status["current_step"] = step_number
    status["completed_steps"].append({
        "step": step_number,
        "description": description,
        "timestamp": datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    })
    
    with open(status_file, "w") as f:
        json.dump(status, f, indent=2, ensure_ascii=False)
    
    # 로그 파일에도 기록
    with open(f"{project_path}/project_log.txt", "a") as f:
        f.write(f"[{datetime.now()}] Step {step_number}: {description}\n")
```

## 📋 **단계별 체크리스트 템플릿**

### **1단계: 주제 조사 체크리스트**
- [ ] 관련 키워드 최소 10개 이상 수집
- [ ] 인기 콘텐츠 3-5개 분석 완료
- [ ] 타겟 오디언스 명확히 정의
- [ ] 경쟁 콘텐츠 차별화 포인트 도출
- [ ] research_summary.md 작성 완료

### **2단계: 대본 구조 설계 체크리스트**
- [ ] 도입-본론-마무리 구조 확정
- [ ] 각 섹션별 시간 배분 완료
- [ ] 핵심 메시지 3개 이하로 압축
- [ ] 콜투액션 문구 결정
- [ ] script_outline.md 작성 완료

### **3단계: 상세 대본 작성 체크리스트**
- [ ] 자연스러운 말투로 작성
- [ ] 음성 합성에 적합한 문장 구조
- [ ] 어려운 단어/발음 확인
- [ ] 전체 읽기 시간 60초 내외
- [ ] script.txt 최종 검토 완료

## 🎯 **품질 관리 템플릿**

### **음성 품질 체크리스트**
- [ ] 발음이 명확하고 자연스러운가?
- [ ] 적절한 속도와 억양인가?
- [ ] 배경 노이즈가 없는가?
- [ ] 볼륨 레벨이 일정한가?
- [ ] 전체 길이가 적절한가?

### **이미지 품질 체크리스트**
- [ ] 9:16 비율 정확히 맞음
- [ ] 1080x1920 해상도 준수
- [ ] 대본 내용과 일치
- [ ] 시각적으로 매력적
- [ ] 플랫폼별 가이드라인 준수

### **최종 영상 품질 체크리스트**
- [ ] 음성과 영상 동기화 완벽
- [ ] 자막 크기와 위치 적절
- [ ] 색감과 밝기 최적화
- [ ] 전환 효과 부드러움
- [ ] 각 플랫폼 사양 준수

## 🚀 **배포 준비 템플릿**

### **플랫폼별 메타데이터**
```json
{
  "youtube_shorts": {
    "title": "[제목 60자 이내]",
    "description": "#Shorts #[주제] #[키워드]",
    "tags": ["태그1", "태그2", "태그3"]
  },
  "instagram_reels": {
    "caption": "[캡션 2200자 이내]",
    "hashtags": ["#릴스", "#[주제]", "#정보"],
    "location": "Seoul, South Korea"
  },
  "tiktok": {
    "description": "[설명 150자 이내]",
    "hashtags": ["#fyp", "#[주제]", "#정보"],
    "effects": ["효과1", "효과2"]
  }
}
```

## 📈 **성과 분석 템플릿**

### **KPI 추적표**
| 플랫폼 | 조회수 | 좋아요 | 댓글 | 공유 | 완주율 |
|--------|-------|-------|------|------|-------|
| 유튜브 | 0 | 0 | 0 | 0 | 0% |
| 인스타 | 0 | 0 | 0 | 0 | 0% |
| 틱톡 | 0 | 0 | 0 | 0 | 0% |

### **개선점 기록**
- **다음에 시도할 것**: 
- **피해야 할 것**: 
- **성공 요인**: 
- **실패 요인**: 
