# Video 相关处理

## 1. 加载某一帧作为poster

```javascript
!function() {

    async function getVideoBase64(url: string, second: number = 0) {
        const video = document.createElement('video');

        video.setAttribute('crossOrigin', 'anonymous'); // CORS设置
        video.setAttribute('src', url);
        video.setAttribute('muted', 'muted'); // 静音操作，防止播放失败
        video.addEventListener('loadeddata', async () => {
            const canvas = document.createElement('canvas');
            const { width, height } = video; // canvas的尺寸和图片一样

            canvas.width = width;
            canvas.height = height;

            if (!!second) {
                video.currentTime = second; // 播放到当前时间的帧，才能截取到当前的画面
                await video.play();
                await video.pause();
            }

            canvas.getContext('2d')?.drawImage(video, 0, 0, width, height);
            return canvas.toDataURL('image/jpeg');
        });
    }

}();
```