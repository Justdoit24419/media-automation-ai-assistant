# 📱 플랫폼별 최적화 가이드

## 🎯 **플랫폼별 핵심 사양**

| 구분 | 유튜브 숏츠 | 인스타그램 릴스 | TikTok | 네이버 TV |
|------|------------|---------------|--------|----------|
| **해상도** | 1080x1920 (9:16) | 1080x1920 (9:16) | 1080x1920 (9:16) | 1080x1920 (9:16) |
| **최대 길이** | 60초 | 90초 | 60초 | 60초 |
| **권장 길이** | 15-30초 | 15-30초 | 15-30초 | 30-45초 |
| **파일 크기** | 최대 15GB | 최대 4GB | 최대 287MB | 최대 2GB |
| **프레임 레이트** | 30fps | 30fps | 30fps | 30fps |
| **비트레이트** | 2-5 Mbps | 3.5 Mbps | 1-3 Mbps | 2-4 Mbps |

## 🔴 **유튜브 숏츠 최적화**

### **🎯 어텐션 전략**
- **첫 3초**: 강력한 훅(Hook) 필수
- **제목**: 궁금증 유발하는 질문형
- **썸네일**: 밝고 대비가 강한 이미지
- **자막**: 24pt, 하단 중앙 배치

### **📊 콘텐츠 구조**
```
0-3초: 강력한 훅 (What, Why, How 질문)
3-15초: 문제 제기 및 관심 유발
15-45초: 핵심 정보 3가지 포인트
45-60초: 콜투액션 (구독, 좋아요, 댓글)
```

### **🛠️ 기술적 최적화**
```bash
# 유튜브 숏츠 최적 인코딩
ffmpeg -i input.mp4 \
  -c:v libx264 -preset medium -crf 23 \
  -c:a aac -b:a 128k \
  -vf "scale=1080:1920:force_original_aspect_ratio=decrease,pad=1080:1920:(ow-iw)/2:(oh-ih)/2" \
  -r 30 -pix_fmt yuv420p youtube_shorts.mp4
```

## 📸 **인스타그램 릴스 최적화**

### **🎨 시각적 특성**
- **미적 감각**: 트렌디하고 세련된 비주얼
- **색감**: 밝고 채도 높은 색상 선호
- **필터**: 자연스러운 보정 효과
- **자막**: 26pt, 스타일리시한 폰트

### **📱 사용자 행동 패턴**
```
빠른 스크롤 → 즉시 어텐션 필요
시각적 임팩트 → 첫 프레임이 결정적
음악 중요도 → 트렌딩 음악 활용
해시태그 → 적절한 태그 활용
```

### **🛠️ 기술적 최적화**
```bash
# 인스타그램 릴스 최적 인코딩
ffmpeg -i input.mp4 \
  -c:v libx264 -preset slow -crf 20 \
  -c:a aac -b:a 128k \
  -vf "scale=1080:1920:force_original_aspect_ratio=decrease,pad=1080:1920:(ow-iw)/2:(oh-ih)/2,eq=contrast=1.1:brightness=0.05:saturation=1.2" \
  -r 30 -pix_fmt yuv420p instagram_reels.mp4
```

## 🎵 **TikTok 최적화**

### **⚡ 에너지와 템포**
- **빠른 편집**: 1.5초마다 새로운 자극
- **음악 동기화**: 비트에 맞춘 컷 편집
- **트렌드 활용**: 최신 챌린지/음악 활용
- **자막**: 22pt, 콤팩트하고 임팩트 있게

### **🎪 콘텐츠 패턴**
```
0-1초: 즉시 임팩트 (효과음 + 시각적 충격)
1-10초: 빠른 전개 (문제 제기)
10-30초: 핵심 내용 (솔루션/정보)
30-45초: 클라이맥스 (재미/놀라움)
45-60초: 마무리 (팔로우 유도)
```

### **🛠️ 기술적 최적화**
```bash
# TikTok 최적 인코딩
ffmpeg -i input.mp4 \
  -c:v libx264 -preset medium -crf 25 \
  -c:a aac -b:a 96k \
  -vf "scale=1080:1920:force_original_aspect_ratio=decrease,pad=1080:1920:(ow-iw)/2:(oh-ih)/2" \
  -r 30 -pix_fmt yuv420p -maxrate 3M -bufsize 3M tiktok.mp4
```

## 📺 **네이버 TV 최적화**

### **🇰🇷 한국 시장 특성**
- **정보성**: 실용적이고 도움되는 콘텐츠
- **자막**: 한글 가독성 최우선
- **길이**: 30-45초 권장 (약간 길어도 OK)
- **톤앤매너**: 진부하고 신뢰할 수 있는 느낌

### **📋 콘텐츠 구조**
```
0-5초: 명확한 주제 소개
5-20초: 문제/상황 설명
20-40초: 해결책/정보 제공
40-60초: 요약 및 채널 소개
```

## 🎛️ **통합 최적화 워크플로우**

### **1단계: 마스터 파일 생성**
```bash
# 최고 품질의 마스터 파일 생성
ffmpeg -i input.mp4 \
  -c:v libx264 -preset slow -crf 18 \
  -c:a flac \
  -vf "scale=1080:1920:force_original_aspect_ratio=decrease,pad=1080:1920:(ow-iw)/2:(oh-ih)/2" \
  master.mov
```

### **2단계: 플랫폼별 배치 변환**
```bash
#!/bin/bash

# 유튜브 숏츠
ffmpeg -i master.mov -c:v libx264 -preset medium -crf 23 -c:a aac -b:a 128k youtube_shorts.mp4

# 인스타그램 릴스
ffmpeg -i master.mov -c:v libx264 -preset slow -crf 20 -c:a aac -b:a 128k -vf "eq=contrast=1.1:brightness=0.05:saturation=1.2" instagram_reels.mp4

# TikTok
ffmpeg -i master.mov -c:v libx264 -preset medium -crf 25 -c:a aac -b:a 96k -maxrate 3M -bufsize 3M tiktok.mp4

# 네이버 TV
ffmpeg -i master.mov -c:v libx264 -preset medium -crf 22 -c:a aac -b:a 128k naver_tv.mp4
```

## 📊 **성과 측정 가이드**

### **핵심 지표**
| 플랫폼 | 주요 지표 | 목표값 | 최적화 포인트 |
|--------|----------|--------|---------------|
| **유튜브** | 조회수, 시청시간 | 70% 완주율 | 훅, 자막, 썸네일 |
| **인스타** | 좋아요, 저장 | 5% 저장율 | 비주얼, 정보성 |
| **틱톡** | 완주율, 공유 | 80% 완주율 | 템포, 음악, 트렌드 |
| **네이버** | 조회수, 댓글 | 긴 시청시간 | 정보 품질, 신뢰성 |

## 🎯 **A/B 테스트 가이드**

### **테스트 요소**
- **썸네일**: 2-3가지 버전
- **제목**: 궁금증 vs 직설적
- **첫 3초**: 다른 훅 방식
- **음악**: 트렌딩 vs 클래식
- **자막 스타일**: 색상, 크기, 위치
