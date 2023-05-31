<template>
    <div class="home-page">
        <div class="hoverPage" @click = "handle">
            <div class="hoverslide">Exotic</div>
            <div class="hoverbottom">监控平台</div>
            <Loading v-if="naviShow" class="naviload"/>
        </div>
        <vue-particles
                color="#30539D"
                :particleOpacity="0.7"
                :particlesNumber="60"
                shapeType="circle"
                :particleSize="6"
                linesColor="#409eff"
                :linesWidth="1"
                :lineLinked="true"
                :lineOpacity="0.4"
                :linesDistance="150"
                :moveSpeed="4"
                :hoverEffect="true"
                hoverMode="grab"
                :clickEffect="true"
                clickMode="push"
                class="back">
            </vue-particles>
        <div class="centerBox">
            <div class="logo"></div>
            <div class="box" >
                <div class="name-top">基于自监督学习的</div>
                <div class="name-bottom" >Web数据智能管理系统</div>
            </div>
            <div class="textarea">
                <write-sql
                    ref="changeSql"
                    @input="input"
                    :valueC="data_value"
                ></write-sql>
            </div>
            <div class="button" @click = "hoverPage">完成</div>
        </div>
        <div class="hover" v-if="hoverShow">
            <div class="mask" @click = "hoverClose"></div>
            <div class="modal">
                <Loading v-if="LoadingShow"/>
                <div class="contanier" v-if="!LoadingShow">
                    <div class="content">{{ renderData }}</div>
                    <!-- <div class="content"  v-for="item,index in renderList" :key = "index">{{ item }}</div> -->
                </div>
            </div>
            <router-link to="/screen" style="text-decoration: none;" class="navigator"><div class="button longer" @click = "hoverPage">前往数据可视化界面</div></router-link>
        </div>
    </div>
    
</template>
  
<script>
import axios from 'axios';
import Loading from '../components/Loading.vue'
import writeSql from '../components/writeSql.vue'
  export default {
    components: {
      Loading,
      writeSql,
    },
    data() {
      return {
        hoverShow: false,
        naviShow: false,
        LoadingShow: true,
        renderData: '',
        renderList:[],
        data_value: '',
        data : "select\ndom_base_uri(dom) as `url`,\ndom_first_text(dom, &apos;#productTitle&apos;) as `title`,\nstr_substring_after(dom_first_href(dom, &apos;#wayfinding-breadcrumbs_container ul li:last-child a&apos;), &apos;&node=&apos;) as `category`,\ndom_first_slim_html(dom, &apos;#bylineInfo&apos;) as `brand`,\ncast(dom_all_slim_htmls(dom, &apos;#imageBlock img&apos;) as varchar) as `gallery`,\ndom_first_slim_html(dom, &apos;#landingImage, #imgTagWrapperId img, #imageBlock img:expr(width > 400)&apos;) as `img`,\ndom_first_text(dom, &apos;#price tr td:contains(List Price) ~ td&apos;) as `listprice`,\ndom_first_text(dom, &apos;#price tr td:matches(^Price) ~ td&apos;) as `price`,\nstr_first_float(dom_first_text(dom, &apos;#reviewsMedley .AverageCustomerReviews span:contains(out of)&apos;), 0.0) as `score`\nfrom load_out_pages(&apos; https://www.amazon.com/b?node=3117954011&apos;, &apos;a[href~=/dp/]&apos;, 1, 10);",
      };
    },
    mounted() {
        axios.get(`//${location.host}/static/template.json`).then((res) => {
            this.renderData = JSON.stringify(res.data[0])
            // console.log('render',JSON.stringify(res.data[0]));
        })
    },
    methods: {
        async hoverPage() {
            this.hoverShow = true
            // var config = {
            //     method: 'post',
            //     url: 'http://118.31.54.166:8182/api/x/e',
            //     headers: { 
            //         'Access-Control-Request-Headers': 'text/plain', 
            //     },
            //     data : this.data
            // };
            // this.renderList = response.split('\n')
            // await axios(config)
            //     .then(function (response) {
            //     console.log(JSON.stringify(response.data));
            //     this.renderData = response.data
            //     this.renderList = response.data.slice('\n')
            //     console.log('renderList',this.renderList);
            //     return response.split('\n')
            //     })
            //     .catch(function () {
            //         let response = 'render\n1\n1\n1\n1\n1\n1\n1\n1\n1\n1\n1\n1\n22\n44\n55\n66\n67567\n8\n\n7k\n7\n'
            //         return response.split('\n')
            // });
            setTimeout(() => {
                this.LoadingShow = false;
            }, 4000);
        },
        handle() {
            console.log('navi');
            document.querySelector('.hoverslide').classList.add('click')
            document.querySelector('.hoverbottom').classList.add('click')
            setTimeout(() => {
                this.naviShow = true
            }, 400);
            
            setTimeout(()=>{
                window.location.href = "http://data.platonic.fun/exotic/crawl/"
                this.naviShow = false
            },2000)
        },
        input(e) {
            console.log(e);
            this.data_value = e
        },
        hoverClose() {
            this.hoverShow = false
            this.LoadingShow = true;
        }
    },
  };
</script>
  
<style lang="less" scoped>
@font-face {
  font-family: 'CustomFont';
  src: url('../assets/font/ZhengQingKeLengKuTi-2.ttf');
  /* 如果字体文件在其他路径下，请根据实际路径进行调整 */
}
    ::-webkit-scrollbar {
    width: 8px;
    height: 8px;
    height: 60px;
    }

    /* 滑块样式 */
    ::-webkit-scrollbar-thumb {
    background-color: #808080;
    border-radius: 10px;
    }

    /* 滑块悬停时的样式 */
    ::-webkit-scrollbar-thumb:hover {
    background-color: #999;
    }

    /* 滑道样式 */
    ::-webkit-scrollbar-track {
    background-color: #f7f7f7;
    display: none;
    }
    .back {
        width: 100vw;
        height: 100vh;
        position: absolute;
        top: 0;
        left: 0;
        z-index: -1;
    }
    @keyframes appear {
        0% {
            scale: 0;
            opacity: 0;
        }
        100% {
            scale: 1;
            opacity: 1;
            
        }
    }
    .naviload {
        position: absolute;
        top: 42%;
        left:50%;
        transform: translate(-50%,-50%);
        z-index: 20001;
    }
    .click {
        width: 50vw !important;
        z-index: 20000 !important;
        background-color: #f0f0f0 !important;
        color: #30539D !important;
        font-size: 40px !important;
    }
    .mask {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        z-index: 100;
    }
    .box {
        .name-top {
            text-align: center;
            font-size: 36px;
            margin-top: 0;
            // font-size: 40px;
            line-height: 36px;
            color: #30539D;
            background-color: #fff;
            font-family: 'CustomFont', sans-serif;
        }
        .name-bottom {
            text-align: center;
            margin-top: 20px;
            font-size: 60px;
            line-height: 60px;
            color: #30539D;
            background-color: #fff;
            font-family: 'CustomFont', sans-serif;
            font-weight: bold;
        }
    }
    .hoverPage {
        font-family: 'CustomFont', sans-serif;
        &:hover {
            .hoverslide {
                height: 100%;
                line-height: 100vh;
                border-radius: 0 0;
            }
            .hoverbottom {
                height: 100%;
                line-height: 100vh;
                border-radius: 0 0;
            }
        }
        .hoverslide {
            position: absolute;
            top: 0;
            right: 0;
            width: 100px;
            height: 6vh;
            border-radius: 0 0  40px 40px;
            background-color: #30539D;
            line-height: 6vh;
            color: #fff;
            text-align: center;
            transition: all .4s ease-in-out;
        }
        .hoverbottom {
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100px;
            height: 6vh;
            line-height: 6vh;
            border-radius: 40px 40px 0 0;
            background-color: #30539D;
            color: #fff;
            text-align: center;
            transition: all .4s ease-in-out;
        }
    }
    .modal {
        animation: appear .3s ease-in-out forwards;
        position: absolute;
        top: 50%;
        left: 50%;
        width: 62%;
        height: 92%;
        transform: translate(-50%, -50%);
        // background: url('../../public/static/template.json');
        // display: flex;
        backdrop-filter: blur(5px);
        background: rgba(255, 255, 255, 0.9);
        color: #000;
        padding: 20px;
        z-index: 101 !important;
        overflow: auto;
        border-radius: 40px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2), 0 4px 8px rgba(0, 0, 0, 0.2);
        &::-webkit-scrollbar {
            display: none;
        }
        .content {
            margin: 10px auto;
            &::-webkit-scrollbar {
                display: none;
            }
            width: 800px;
            overflow: auto;
        }
    }
    .button{
        // animation: appear .4s ease-in-out forwards;
        margin-top: 30px;
        color: #30539D;
        cursor: pointer;
        background-color: #fff;
        width: 80px;
        height: 40px;
        line-height: 40px;
        text-align: center;
        border-radius: 10px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2), 0 4px 8px rgba(0, 0, 0, 0.2);
        &:hover {
            animation: color .3s ease-in-out forwards;
        }
        &:active {
            animation: colorReverse .3s ease-in-out forwards;
        }
    }
    @keyframes color {
        0% {
            opacity: 1;
            color: #30539D;
            background-color: #fff;
        }
        100% {
            opacity: 1;
            color: #fff;
            background-color: #30539D;
        }
    }
    @keyframes colorReverse {
        0% {
            color: #fff;
            background-color: #30539D;
        }
        100% {
            color: #fff;
            background-color: #083fff;
        }
    }
    .navigator {
        position: absolute;
        bottom: 50px;
        left:50%;
        transform: translateX(-50%);
        z-index: 103;
    }
    .longer {
        width: 200px;
    }
    .home-page {
        display: flex;
        justify-content: center;
        align-items: center;
        margin: 0 auto;
        .centerBox {
            display: flex;
            flex-direction: column;
            align-items: center;
            align-content: center;
            // margin-top: 20px;
            
            .logo {
                width: 400px;
                height: 160px;
                background: url('../assets/logof.png') no-repeat;
                background-size: 400px,200px;
            }
        }
        .textarea {
            margin-top: 50px;
            appearance: none;
            border-radius: 10px;
            outline: none;
            border: 0;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2), 0 4px 8px rgba(0, 0, 0, 0.2);
            font-size: 16px;
            padding: 10px;
            width: 600px;
            height: 200px;
            resize: none;
        }
    }
</style>