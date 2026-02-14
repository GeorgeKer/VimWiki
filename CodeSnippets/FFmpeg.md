# FFmpeg CLI Commands And Snippets

## avpres.net - 'FFmpeg Cookbook for Archivists'

## **Filter Image**
- **4:3 → 16:9 (pillarbox, no scaling)**
`ffmpeg -i input_file -filter:v "pad=ih*16/9:ih:(ow-iw)/2:(oh-ih)/2" -c:a copy output_file`

- **16:9 → 4:3 (letterbox, no scaling)**
`ffmpeg -i input_file -filter:v "pad=iw:iw*3/4:(ow-iw)/2:(oh-ih)/2" -c:a copy output_file`  

- **Extract image (video) stream only**
`ffmpeg -i input_file -c:v copy -an output_file  `

- **Remove image (video), keep all other streams**
`ffmpeg -i input_file -vn -c copy output_file  `

- **Flip horizontally and vertically**
`ffmpeg -i input_file -filter:v "hflip,vflip" -c:a copy output_file 
`
- **Change projection/viewing speed (video only)**
`ffmpeg -i input_file -filter:v "setpts=input_fps/output_fps*PTS" -an output_file  `

## **Filter Sound**
- **Extract audio stream only**
`ffmpeg -i input_file -c:a copy -vn output_file  `

## **Filter Image and Sound**
- **Change AV speed (e.g., 24 fps → 25 fps)**
`ffmpeg -i input_file -r output_fps -filter_complex "[0:v]setpts=input_fps/output_fps*PTS[v];[0:a]atempo=output_fps/input_fps[a]" -map "[v]" -map "[a]" output_file  `

## **Transcode Image**
- **Image sequence → H.264 / AVC**
`ffmpeg -f image2 -framerate 24 -i input_file_%06d.extension -c:v libx264 -preset veryslow -crf 18 -pix_fmt yuv420p output_file  `

- **Video → H.264 / AVC**
`ffmpeg -i input_file -c:v libx264 -preset veryslow -crf 18 -pix_fmt yuv420p -c:a copy output_file  `

## **Transform Container**
- **Matroska (.mkv) → MP4**
`ffmpeg -i input_file.mkv -c:v copy -c:a aac output_file.mp4  `


