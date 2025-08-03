# ⚙️ FFmpeg 다이나믹 편집 명령어 레퍼런스

## 🎬 **기본 다이나믹 편집**

### **Ken Burns 효과 (줌인 + 팬)**
```bash
scale=1200:1200,zoompan=z='zoom+0.002':x='iw/2-(iw/zoom/2)':y='ih/2-(ih/zoom/2)':d=225
```

### **페이드 인/아웃 전환**
```bash
fade=t=in:st=0:d=0.5,fade=t=out:st=6.5:d=0.5
```

### **크로스 디졸브 전환**
```bash
blend=all_mode=dissolve:all_opacity=0.5
```

### **텍스트 애니메이션**
```bash
drawtext=text='핵심메시지':x='(w-text_w)/2':y='h-200':fontsize=48:fontcolor=white:enable='between(t,1,4)'
```

## 🔥 **고급 다이나믹 편집**

### **확대 + 회전 + 페이드**
```bash
scale=1200:1200,rotate=PI/180*t,zoompan=z='1+0.002*t':d=225,fade=t=in:st=0:d=0.3
```

### **색상 강화 + 콘트라스트**
```bash
colorbalance=rs=0.1:gs=0.1:bs=0.1,eq=contrast=1.2:brightness=0.1
```

### **다중 텍스트 애니메이션**
```bash
drawtext=text='매일 야근?':x='(w-text_w)/2':y='200':fontsize=36:fontcolor=red:enable='between(t,0,2)':alpha='if(between(t,0,0.5),(t-0)/0.5,if(between(t,1.5,2),(2-t)/0.5,1))'
```

## 🎵 **오디오 처리 명령어**

### **배경음악 볼륨 조절**
```bash
ffmpeg -i voice.mp3 -i bgm.mp3 -filter_complex "[1:a]volume=0.3[bg];[0:a][bg]amix=inputs=2:duration=first:dropout_transition=3" output.mp3
```

### **오디오 노이즈 제거**
```bash
ffmpeg -i input.mp3 -af "highpass=f=85,lowpass=f=8000" output.mp3
```

### **오디오 정규화**
```bash
ffmpeg -i input.mp3 -af loudnorm=I=-16:TP=-1.5:LRA=11 output.mp3
```

## 🖼️ **이미지 처리 명령어**

### **이미지를 비디오로 변환 (Ken Burns 효과)**
```bash
ffmpeg -loop 1 -i image.jpg -vf "scale=1080:1920:force_original_aspect_ratio=decrease,pad=1080:1920:(ow-iw)/2:(oh-ih)/2,zoompan=z='min(zoom+0.0015,1.5)':d=125" -t 5 -pix_fmt yuv420p output.mp4
```

### **여러 이미지를 하나의 비디오로 결합**
```bash
ffmpeg -f concat -safe 0 -i filelist.txt -vf "scale=1080:1920:force_original_aspect_ratio=decrease,pad=1080:1920:(ow-iw)/2:(oh-ih)/2" -c:v libx264 -pix_fmt yuv420p output.mp4
```

### **이미지에 자막 추가**
```bash
ffmpeg -i image.jpg -vf "drawtext=text='자막 텍스트':fontfile=/path/to/font.ttf:fontsize=48:fontcolor=white:x=(w-text_w)/2:y=h-200" output.jpg
```

## 🎬 **완성된 숏폼 영상 생성 명령어**

### **이미지 + 음성 + 배경음악 결합**
```bash
ffmpeg -f concat -safe 0 -i images.txt -i voice.mp3 -i bgm.mp3 \
  -filter_complex "[0:v]scale=1080:1920:force_original_aspect_ratio=decrease,pad=1080:1920:(ow-iw)/2:(oh-ih)/2,zoompan=z='min(zoom+0.002,1.5)':d=125[v]; \
                   [2:a]volume=0.3[bg]; \
                   [1:a][bg]amix=inputs=2:duration=first:dropout_transition=3[a]" \
  -map "[v]" -map "[a]" -c:v libx264 -c:a aac -pix_fmt yuv420p -t 60 output.mp4
```

### **자막이 포함된 최종 영상**
```bash
ffmpeg -i video.mp4 -vf "subtitles=subtitles.srt:force_style='Fontname=AppleSDGothicNeo-Bold,Fontsize=24,PrimaryColour=&Hffffff,OutlineColour=&H000000,Outline=2'" final_output.mp4
```

## 📊 **품질 최적화 설정**

### **고품질 인코딩 설정**
```bash
-c:v libx264 -preset slow -crf 18 -pix_fmt yuv420p
```

### **파일 크기 최적화**
```bash
-c:v libx264 -preset medium -crf 23 -pix_fmt yuv420p
```

### **빠른 인코딩 (테스트용)**
```bash
-c:v libx264 -preset ultrafast -crf 28 -pix_fmt yuv420p
```

## 🚀 **자동화 스크립트 예제**

### **전체 워크플로우 자동화**
```bash
#!/bin/bash

# 1. 이미지 전처리
for img in images/*.png; do
  ffmpeg -i "$img" -vf "scale=1080:1920:force_original_aspect_ratio=decrease,pad=1080:1920:(ow-iw)/2:(oh-ih)/2" "processed_$(basename "$img")"
done

# 2. 음성 + 배경음악 믹싱
ffmpeg -i voice.mp3 -i bgm.mp3 -filter_complex "[1:a]volume=0.3[bg];[0:a][bg]amix=inputs=2:duration=first:dropout_transition=3" mixed_audio.mp3

# 3. 최종 영상 생성
ffmpeg -f concat -safe 0 -i filelist.txt -i mixed_audio.mp3 \
  -vf "zoompan=z='min(zoom+0.002,1.5)':d=125" \
  -c:v libx264 -c:a aac -pix_fmt yuv420p final_video.mp4
```

## 🛠️ **디버깅 및 트러블슈팅**

### **진행 상황 모니터링**
```bash
ffmpeg -progress progress.txt -i input.mp4 ... output.mp4
```

### **상세 로그 출력**
```bash
ffmpeg -loglevel debug -i input.mp4 ... output.mp4
```

### **GPU 가속 (Mac M1/M2)**
```bash
ffmpeg -hwaccel videotoolbox -i input.mp4 -c:v h264_videotoolbox output.mp4
```
