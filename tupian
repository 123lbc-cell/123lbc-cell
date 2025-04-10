<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>瀑布流图片展示</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: Arial, sans-serif;
            background-color: #f5f5f5;
            padding: 10px;
        }
        
        .waterfall-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 15px;
        }
        
        .waterfall-item {
            background: white;
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
            transition: transform 0.3s ease;
        }
        
        .waterfall-item:hover {
            transform: translateY(-5px);
        }
        
        .waterfall-item img {
            width: 100%;
            height: auto;
            display: block;
        }
        
        .waterfall-item-info {
            padding: 12px;
        }
        
        .waterfall-item-title {
            font-weight: bold;
            margin-bottom: 5px;
            color: #333;
            font-size: 14px;
        }
        
        .waterfall-item-source {
            font-size: 12px;
            color: #888;
            word-break: break-all;
        }
        
        .loading {
            text-align: center;
            padding: 20px;
            color: #666;
            grid-column: 1 / -1;
        }
        
        @media (max-width: 600px) {
            .waterfall-grid {
                grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            }
        }
    </style>
</head>
<body>
    <div class="waterfall-grid" id="waterfall"></div>
    <div class="loading" id="loading">加载更多图片...</div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const waterfall = document.getElementById('waterfall');
            const loading = document.getElementById('loading');
            let isLoading = false;
            let loadedImages = new Set();
            
            const apiList = [
                {
                    name: '狗狗图片',
                    url: 'https://dog.ceo/api/breeds/image/random',
                    type: 'json-url'
                },
                {
                    name: '随机图片',
                    url: 'https://picsum.photos/',
                    type: 'blob'
                },
                {
                    name: '猫咪图片',
                    url: 'https://api.thecatapi.com/v1/images/search?limit=5',
                    type: 'json-array'
                }
            ];
            
            loadBatchImages();
            
            window.addEventListener('scroll', function() {
                if (isLoading) return;
                
                const scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
                const windowHeight = window.innerHeight;
                const documentHeight = document.documentElement.scrollHeight;
                
                if (scrollTop + windowHeight >= documentHeight - 500) {
                    loadBatchImages();
                }
            });
            
            async function loadBatchImages() {
                if (isLoading) return;
                
                isLoading = true;
                loading.style.display = 'block';
                
                const count = 6 + Math.floor(Math.random() * 6);
                const imagePromises = [];
                
                for (let i = 0; i < count; i++) {
                    imagePromises.push(createImageItem());
                }
                
                const items = await Promise.all(imagePromises);
                items.forEach(item => item && waterfall.appendChild(item));
                
                isLoading = false;
                loading.style.display = 'none';
            }
            
            async function createImageItem() {
                const randomApi = apiList[Math.floor(Math.random() * apiList.length)];
                let imageUrl, imageName;
                
                try {
                    switch (randomApi.type) {
                        case 'json-url':
                            const jsonUrlResponse = await fetch(randomApi.url);
                            const jsonUrlData = await jsonUrlResponse.json();
                            imageUrl = jsonUrlData.message;
                            imageName = `${randomApi.name} - ${imageUrl.split('/').pop()}`;
                            break;
                            
                        case 'blob':
                            const width = 200 + Math.floor(Math.random() * 300);
                            const height = 300 + Math.floor(Math.random() * 300);
                            imageUrl = `${randomApi.url}${width}/${height}?random=${Math.random().toString(36).substring(2)}`;
                            imageName = `${randomApi.name} - ${width}x${height}`;
                            break;
                            
                        case 'json-array':
                            const jsonArrayResponse = await fetch(randomApi.url);
                            const jsonArrayData = await jsonArrayResponse.json();
                            const randomItem = jsonArrayData[Math.floor(Math.random() * jsonArrayData.length)];
                            imageUrl = randomItem.url;
                            imageName = `${randomApi.name} - ${imageUrl.split('/').pop()}`;
                            break;
                    }
                    
                    if (loadedImages.has(imageUrl)) {
                        console.log('跳过已加载的图片:', imageUrl);
                        return createImageItem();
                    }
                    
                    loadedImages.add(imageUrl);
                    
                    const item = document.createElement('div');
                    item.className = 'waterfall-item';
                    item.innerHTML = `
                        <img src="${imageUrl}" alt="${imageName}" loading="lazy" onerror="this.src='https://via.placeholder.com/300x400?text=图片加载失败'">
                        <div class="waterfall-item-info">
                            <div class="waterfall-item-title">${imageName}</div>
                            <div class="waterfall-item-source">来源: ${imageUrl}</div>
                        </div>
                    `;
                    
                    console.log(`图片名称: ${imageName}, URL: ${imageUrl}`);
                    return item;
                } catch (error) {
                    console.error('创建图片失败:', error);
                    const placeholder = document.createElement('div');
                    placeholder.className = 'waterfall-item';
                    placeholder.innerHTML = `
                        <img src="https://via.placeholder.com/300x400?text=图片加载失败" alt="加载失败">
                        <div class="waterfall-item-info">
                            <div class="waterfall-item-title">加载失败的图片</div>
                        </div>
                    `;
                    return placeholder;
                }
            }
        });
    </script>
</body>
</html>
