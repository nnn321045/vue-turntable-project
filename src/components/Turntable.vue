<script>
export default {
    data(){
        return{
            start_rotating_degree: 0, // 初始旋轉角度
            rotate_angle: 0, // 將要旋轉角度
            resultAngle : [0 , 270 , 180 , 90], // 最終會旋轉到的下標位置所需要度數
            rotate_transition: "transform 7s ease-in-out" , // 初始化中獎的過場控制
            click_flag: true , // 是否可以抽獎
            
            // li 的偽元素 css , 指針選到的加上顏色
            liStyle: {
                "--color": "",
                "--color1":"",
                "--color2":"",
                "--color3":"",
            },
    

            prizeListOrigin:[
                {
                    id: 1,
                    name: "body_care_gift"
                },
                {
                    id: 2,
                    name: "face_care_gift"
                },
                {
                    id: 3,
                    name: "healthy_gift"
                },
                {
                    id: 4,
                    name: "special_gift"
                }
            ],

            // 獎品數量
            numberPrize:{
                body_care_gift: 1,
                face_care_gift: 2,
                healthy_gift: 3,
                special_gift: 4
            },
        }
    },

    computed: {
        prizeList () {
            return this.prizeListOrigin.slice(0) // 複製陣列
        }
    },

    methods:{
        rotating(index){
            if(!this.click_flag) return;
            let duringTime = 6; // 默認為 1s
            let random = Math.floor(Math.random() * 6);
            this.click_flag = false; // 旋轉結束前 , 不能再次觸發
            
            let rotateAngle = 
            this.start_rotating_degree + ((random + 8) * 360) + this.resultAngle[index] - (this.start_rotating_degree % 360);

            this.start_rotating_degree = rotateAngle;
            this.rotate_angle = `rotate(${rotateAngle}deg)`;
    
            let self = this;
            
            // 確保輪盤有轉完
            setTimeout(() => {
                self.click_flag = true;
        
                // 中獎信息
                if(this.resultAngle[index] === this.resultAngle[0]) {
                    this.liStyle['--color'] = "red";
                    if(this.numberPrize.body_care_gift === 0){
                        alert('獎品已抽完'); return;
                    }
                    
                    this.numberPrize.body_care_gift = this.numberPrize.body_care_gift -1;
                    alert(`恭喜您抽中了: body_care_gift , 獎品剩餘數量:${this.numberPrize.body_care_gift}`);
                }

                if(this.resultAngle[index] === this.resultAngle[1]){
                    this.liStyle['--color2'] = "red";
                    if(this.numberPrize.healthy_gift === 0){
                        alert('獎品已抽完'); return;
                    }

                    this.numberPrize.healthy_gift = this.numberPrize.healthy_gift -1;
                    alert(`恭喜您抽中了: healthy_gift , 獎品剩餘數量:${this.numberPrize.healthy_gift}`);
                } 

                if(this.resultAngle[index] === this.resultAngle[2]){ 
                    this.liStyle['--color3'] = "red";
                    if(this.numberPrize.special_gift === 0){
                        alert('獎品已抽完'); return;
                    }

                    this.numberPrize.special_gift = this.numberPrize.special_gift -1;
                    alert(`恭喜您抽中了: special_gift , 獎品剩餘數量:${this.numberPrize.special_gift}`)
                };   

                if(this.resultAngle[index] === this.resultAngle[3]){ 
                    this.liStyle['--color1'] = "red";
                    if(this.numberPrize.face_care_gift === 0){
                        alert('獎品已抽完'); return;
                    }

                    this.numberPrize.face_care_gift = this.numberPrize.face_care_gift -1;
                    alert(`恭喜您抽中了: face_care_gift , 獎品剩餘數量:${this.numberPrize.face_care_gift}`);
                }

            }, duringTime * 1000 + 1000);
        },

        startFn(){
            this.rotating(Math.floor(Math.random() * 4));

            // 下次點擊時 , 清空上一次點擊顏色
            this.liStyle['--color'] = '';
            this.liStyle['--color1'] = '';
            this.liStyle['--color2'] = '';
            this.liStyle['--color3'] = '';
        },
    }
}
</script>

<template>
    <div class="turntable-wrap">
        <div class="turntable">
            <ul class="bg">
                <li v-for="item in prizeList" :key="item.id" :style="liStyle">
                    {{item.name}}
                </li>
            </ul>
            <ul class="bg-line">
                <li></li>
                <li></li>
            </ul>
        </div>

        <!-- 指針 -->
        <div class="sector" :style="{transform:rotate_angle , transition:rotate_transition}">
            <div class="sector-inner"></div>
        </div>

        <div class="startBtn" @click="startFn()">Start</div>
    </div>
</template>

<style lang="scss" scoped>
.turntable-wrap {
    position: absolute;
    top:50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 340px;
    height: 340px;
    border: 7px solid #b2a98d;
    border-radius: 50%;  
    box-shadow: 0 0 20px #b2a98d;
    .turntable{
        .bg{
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgb(231, 175, 175);
            border: 1px solid #dfd8be;
            border-radius: 50%;
            overflow: hidden;
            display: flex;
            flex-wrap: wrap;
            li{
                list-style: none;
                width: 50%; 
                font-size: 16px; 
                font-weight: bold;
                color:#fff;
                display: flex;
                justify-content: center;
                align-items: center;
            }
            li:nth-child(1){
                border-radius: 130px 0 0 0;
                background: var(--color);
            }
            li:nth-child(2){
                border-radius: 0 130px 0 0; 
                background: var(--color1);
            }
            li:nth-child(3){
                border-radius: 0 0 0 130px;
                background: var(--color2);
            }
            li:nth-child(4){ 
                border-radius: 0 0 130px 0;
                background: var(--color3);
            }
        }
        .bg-line{
            li{
                position: absolute;
                top: 0;
                left: 0;
                right: 0;
                margin: 0 auto;
                width: 2px;
                height: 100%;
                background: #000;
                transform-origin: center center;
                list-style: none;
                transform: rotate(90deg);
                background: #fff;
            }
            li:nth-child(1){
                transform: rotate(0deg);
            }
        }
    }
    .sector { 
        position: absolute; 
        width: 100%; 
        height: 100%; 
        margin: 10px; 
        .sector-inner { 
            height: 0;
            width: 15px;
            border-bottom: 90px solid #cccccc;
            border-left: 4px solid transparent;
            border-right: 4px solid transparent;
            transform: rotate(123deg);
            position: absolute;
            top: 0;
            left: 0;
        } 
    } 
    .startBtn{
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        border: 1px solid #000;
        border-style: none;
        background: #fff;
        padding: 20px;
        cursor: pointer;
    }
    .startBtn:hover{
        background: #eee;
    }
}
</style>