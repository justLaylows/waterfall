<template>
    <div id="waterfall" ref="waterfall">
        <div v-for="(item, index) in showImages" class="waterfallItem"  :key="index" 
            :style="{
                 width:item.width+'px',
                 height: item.height+'px',
             }">
            <img alt="" :data-src="item.url">
        </div>
    </div>
</template>

<script>

import { debounce } from '../network/debounce'
export default {
    name: 'waterfall',
    props: {
        // 定义图片之间的间距
        imgGrap:{
            type:Number,
            default:10
        },
        // 设置一行排列的图片数量
        columns:{
            type:Number,
            default:5
        },
        // 传入的图片数组，每个子元素至少包含url(图片请求地址)
        imgLists:{
            type:Array,
            default:[{
                "url": "https://w.wallhaven.cc/full/6o/wallhaven-6oxem6.jpg",
            },
            {
                "url": "https://w.wallhaven.cc/full/wq/wallhaven-wq9x27.jpg",
               
            },
            {
                "url": "https://w.wallhaven.cc/full/wq/wallhaven-wq9lk7.jpg",
               
            },
            {
                "url": "https://w.wallhaven.cc/full/wq/wallhaven-wq9ddx.jpg",
              
            },
            {
                "url": "https://w.wallhaven.cc/full/e7/wallhaven-e7kelw.jpg",
               
            },
            {
                "url": "https://w.wallhaven.cc/full/rd/wallhaven-rd88g1.jpg",
               
            },
            {
                "url": "https://w.wallhaven.cc/full/m9/wallhaven-m97128.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/28/wallhaven-2873ey.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/8o/wallhaven-8oq17j.png",
               
            },
            {
                "url": "https://w.wallhaven.cc/full/y8/wallhaven-y8dzpk.jpg",
               
            },
            {
                "url": "https://w.wallhaven.cc/full/6o/wallhaven-6o8e6q.jpg",
               
            },
            {
                "url": "https://w.wallhaven.cc/full/72/wallhaven-728rey.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/v9/wallhaven-v98gq3.jpg",
               
            },
            {
                "url": "https://w.wallhaven.cc/full/3z/wallhaven-3zm3d3.jpg",
               
            },
            {
                "url": "https://w.wallhaven.cc/full/28/wallhaven-28mj69.png",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/l3/wallhaven-l3lrqy.jpg",
               
            },
            {
                "url": "https://w.wallhaven.cc/full/6o/wallhaven-6ox1gx.jpg",
               
            },
            {
                "url": "https://w.wallhaven.cc/full/k7/wallhaven-k7yw91.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/k7/wallhaven-k7yxqm.jpg",
               
            },
            {
                "url": "https://w.wallhaven.cc/full/j3/wallhaven-j329gq.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/g7/wallhaven-g7y3ol.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/m9/wallhaven-m9jve9.png",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/y8/wallhaven-y8d3pd.png",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/y8/wallhaven-y81go7.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/6o/wallhaven-6o81p7.png",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/6o/wallhaven-6oxjv6.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/o3/wallhaven-o3pr99.png",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/1k/wallhaven-1k6m6g.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/28/wallhaven-28mqog.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/pk/wallhaven-pk6l1m.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/rd/wallhaven-rd86vm.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/72/wallhaven-72zxk3.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/q2/wallhaven-q2x3w7.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/l3/wallhaven-l3ljq2.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/x8/wallhaven-x8rvjd.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/72/wallhaven-728rm3.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/o3/wallhaven-o3poe7.jpg",
               
            },
            {
                "url": "https://w.wallhaven.cc/full/o3/wallhaven-o3ogml.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/wq/wallhaven-wq99px.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/q2/wallhaven-q278r7.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/dp/wallhaven-dprdmo.jpg",
                
            },
            {
                "url": "https://w.wallhaven.cc/full/pk/wallhaven-pke5qj.png",
                
            }]
         }
    },
    data() {
        return {
             imgWidth: 0,  //表示每个图片的宽度
            columnHeights: [],//存储每列的最后放置完图片的位置
            boxWidth: 0,    //表示每个图片盒子的宽度，即包含图片宽度与边距
            positionTop: 0,  //每张图片放置的top位置
            showImages:[],   //预加载之后需要展示的图片
            imgBoxs:null,     //用于放置所有装有img的div盒子
            observer:null,   //观察函数，用于懒加载
            debounceResize: null, //防抖函数，用于窗口改变之后，重新布局

        }
    },
    methods: {
        // 观察每张图片，出现在用户视野就开始图片加载
        observerImages(images){
           images.forEach(item=>{
             this.observer.observe(item)
           })
        },
        // IntersectionObserver内部的回调函数
        callback(entries){
            entries.forEach(entry=>{
                if(entry.isIntersecting){  //判断此dom是否与浏览器的视口是否相交
                    const targetDiv=entry.target;  //获取目标dom
                    const image=targetDiv.childNodes[0];
                    const data_src=image.getAttribute('data-src');//获取img中的data-src属性
                    image.setAttribute('src',data_src);  //开始加载
                    this.observer.unobserve(targetDiv); //观察完毕之后就不再重复观察
                   
                }
            })
        },
        //计算或寻找每列中高度最小的位置，用于设置下一张图片的top位置
        getMinHeight() { 
            this.positionTop = Math.min(...this.columnHeights)
            return this.columnHeights.indexOf(this.positionTop)
        },
        // 初始化基本数据
        initData() {
            this.imgWidth = this.$refs.waterfall.offsetWidth / this.columns - this.imgGrap;
            this.boxWidth = this.imgWidth + this.imgGrap;
            this.columnHeights = new Array(this.columns).fill(0);

        },
        // 预加载图片
        preLoad(images) {
            const imgs = []
            images.forEach(item => {
              imgs.push(this.loadImage(item.url)) ; 
            })
            //  Promise.all检查所有图片是否加载完成
            return Promise.all(imgs) 
        },
        loadImage(src) {
            // 因为new的关键字，Promise中this指向改变，所有记录下vuecomponent，用于获取imgWidth
            const that=this
            return new Promise(function (resolve, reject) {
                const img = new Image();
               img.onerror=img.onload = () =>{//加载完成或失败执行resolve函数
                    resolve({
                        url:src,
                        width:that.imgWidth,
                        height:img.width==0?0:that.imgWidth*img.height/img.width,//避免除0情况
                       
                    });
                    
                }
                img.src = src;
            })
        },
        //开始瀑布流布局
        waterfallLayOut(){ 
           for(let i=0;i<this.imgBoxs.length;i++){

                const minHeightIndex = this.getMinHeight(); //获取所有列中高度最小的索引
              this.imgBoxs[i].style.top= this.positionTop + 'px'  //设置每个imgBox的相对定位位置
            this.imgBoxs[i].style.left = minHeightIndex*this.boxWidth+'px'
        this.columnHeights[minHeightIndex] += this.showImages[i].height + this.imgGrap; //更新相应列的高度

           }
        },
        //当窗口改变，重新计算数据与布局
        reLayout(){
            const oldWidth=this.imgWidth; //获取之前每个图片的宽度
            this.initData();  //重新初始化数据
            if(this.imgWidth===oldWidth) return //如果宽度没有改变，就不必重新布局
            for(let item of this.showImages){  //重新计算图片宽度与高度
                item.height=this.imgWidth*item.height/item.width; 
                item.width=this.imgWidth;
            }
            this.waterfallLayOut() //计算完图片高度与宽度之后，再次布局

        }

    }
    ,
    mounted() {
        this.debounceResize = debounce(this.reLayout) 
        //初始化观察函数
        this.observer=new IntersectionObserver(this.callback)

        this.initData()  //初始化化数据
        // console.log(this.imgWidth)
        //所有图片预加载完成，将结果更新至展示数组中
        this.preLoad(this.imgLists).then(res=>{
         this.showImages.push(...res) 
         this.$nextTick(()=>{
            //获取所有装右图片的div盒子
            this.imgBoxs=this.$el.getElementsByClassName('waterfallItem')
             //依次观察每个div盒子，出现在用户视野处就加载图片
            this.observerImages(Array.from(this.imgBoxs))
            //预加载完图片后，就可以瀑布流布局
            this.waterfallLayOut()
            //监听浏览器窗口大小是否改变，用于重新布局
            window.addEventListener('resize', this.debounceResize)
        })
          
       })
        

    },
    beforeDestroy(){
        window.removeEventListener('resize',this.debounceResize)
    }

}
</script>


<style scoped>
#waterfall {
    width: 100%;
    height: 100%;
    position: relative;
}

.waterfallItem {

    position: absolute;
    border: 1px solid rgb(24, 24, 24);
    animation: show-img .5s ease;

}

.waterfallItem img {
    width: 100%;
    height: 100%;

}

@keyframes show-img {
      0% {
        transform: scale(0.5);
      }
      100% {
        transform: scale(1);
      }
}
</style>