<template>
    <div class="ratings" ref="ratings">
        <div class="rating-content">
            <div class="overview">
                <div class="overview-left">
                    <h1 class="score">{{ seller.score}}</h1>
                    <div class="title">综合评分</div>
                    <div class="rank">高于周边商家{{seller.rankRate}}%</div>
                </div>
                <div class="overview-right">
                    <div class="score-wrapper">
                        <span class="title">服务态度</span>
                       <star :size="36" :score="seller.serviceScore"></star>
                        <span class="score">{{seller.serviceScore}}</span>
                    </div>
                    <div class="score-wrapper">
                        <span class="title">商品评分</span>
                       <star :size="36" :score="seller.serviceScore"></star>
                        <span class="score">{{seller.foodScore}}</span>
                    </div>
                    <div class="delivery-wrapper">
                        <span class="title">送达时间</span>
                        <span class="delivery">{{seller.deliveryTime}}分钟</span>
                    </div>
                </div>
            </div>
            <div class="split"></div>
            <ratingselect 
                :selectType="selectType" 
                :onlyContent="onlyContent"
                :ratings="ratings"
                @select="selectRating"
                @toggle='toggleContent'
            ></ratingselect>
            <div class="rating-wrapper">
                <ul>
                    <li v-for="(rating,index) in ratings" :key="rating.index" v-show="needShow(rating.rateType, rating.text)" class="rating-item">
                        <div class="avatar">
                        <img width="28" height="28" :src="rating.avatar">
                        </div>
                        <div class="content">
                        <h1 class="name">{{rating.username}}</h1>
                        <div class="star-wrapper">
                            <star :size="24" :score="rating.score"></star>
                            <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}</span>
                        </div>
                        <p class="text">{{rating.text}}</p>
                        <div class="recommend" v-show="rating.recommend && rating.recommend.length">
                            <span class="icon-thumb_up"></span>
                            <span class="item"  v-for="(item,index) in rating.recommend" :key="item.index">{{item}}</span>
                        </div>
                        <div class="time">
                            {{rating.rateTime | formatDate}}
                        </div>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
import BScroll from 'better-scroll'
import star from 'components/star/star'
import ratingselect from 'components/ratingselect/ratingselect'
import axios from 'axios'
import {formatDate} from 'common/js/date';
const ERR_OK = 'OK';
const ALL = 2;
export default {
    name:'ratings',
    data () {
        return {
            ratings:[],
            seller:{},
            selectType: ALL,
            onlyContent: true
        }
    },
    components: {
        star,
        ratingselect
    },
    mounted () {
        axios.get('/api/data.json').then(this.getData);
    },
    methods: {
        getData(res){
            if(res.statusText === ERR_OK){
                res = res.data
                this.seller = res.seller
                this.ratings = res.ratings
                console.log(this.ratings)
                this.$nextTick(() => {
                    this.scroll = new BScroll(this.$refs.ratings, {
                        click: true
                    });
                });
            }
        },
        needShow(type, text){
            if (this.onlyContent && !text) {
                return false;
            }
            if (this.selectType === ALL) {
                return true;
            } else {
                return type === this.selectType;
            }
        },
        selectRating(type){
            this.selectType = type;
            this.$nextTick(() => {
                this.scroll.refresh();
            });
        },
        toggleContent() {
        this.onlyContent = !this.onlyContent;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
      }
    },
    filters: {
      formatDate(time) {
        let date = new Date(time);
        return formatDate(date, 'yyyy-MM-dd hh:mm');
      }
    }, 
}
</script>

<style lang="stylus" scoped>
    @import "../../common/stylus/mixin.styl"
    .ratings
        position absolute 
        top 174px
        bottom 0 
        left 0
        width 100%
        overflow hidden
        .overview
            padding 18px 0
            display flex
            .overview-left
                flex 0 0 137px
                padding 6px 0
                width 137px
                border-right 1px solid rgba(7,17,27,.1)
                text-align center
                flex 0 0 120px
                width 120px
                .score
                    color #f90
                    font-size 24px
                    line-height 24px
                    margin-bottom 6px
                .title
                    color #07111b
                    font-size 12px
                    line-height 12px
                    margin-bottom 6px
                .rank
                    line-height 10px
                    font-size 10px
                    color #93999f
            .overview-right
                padding 6px 0 6px 24px
                flex 1
                padding-left 6px
                .score-wrapper
                    margin-bottom 8px 
                    font-size 0
                    .title
                        display inline-block
                        vertical-align top
                        font-size 12px
                        line-height  18px
                        color #07111b
                    .star
                        display  inline-block
                        margin 0 12px
                        vertical-align  top
                    .score
                        display inline-block
                        line-height 18px
                        vertical-align top 
                        font-size 12px
                        color #f90
                .delivery-wrapper
                    font-size 0
                    .title
                        font-size  12px
                        line-height 18px
                        color #07111b
                    .delivery
                        margin-left 12px
                        font-size 12px
                        color #93999f
        .split
            width: 100%
            height: 16px
            border-top: 1px solid rgba(7, 17, 27, 0.1)
            border-bottom: 1px solid rgba(7, 17, 27, 0.1)
            background: #f3f5f7
        .rating-wrapper
            padding 0 18px
            .rating-item 
                padding 18px 0
                display  flex
                border-1px(rgba(7, 17, 27, 0.1))
                .avatar
                    flex 0 0 28px
                    margin-right 12px
                    width 28px
                .content 
                    flex 1
                    position relative
                    .name
                        margin-bottom: 4px
                        line-height: 12px
                        font-size: 10px
                        color: rgb(7, 17, 27)
                    .star-wrapper
                        margin-bottom: 6px
                        font-size: 0
                        .star
                            display: inline-block
                            margin-right: 6px
                            vertical-align: top
                        .delivery
                            display: inline-block
                            vertical-align: top
                            line-height: 12px
                            font-size: 10px
                            color: rgb(147, 153, 159)
                    .text
                        margin-bottom: 8px
                        line-height: 18px
                        color: rgb(7, 17, 27)
                        font-size: 12px
                    .recommend
                        line-height: 16px
                        font-size: 0
                        .icon-thumb_up
                            color: #00a0dc
                        .item
                            padding: 0 6px
                            border: 1px solid rgba(7, 17, 27, 0.1)
                            border-radius: 1px
                            color: rgb(147, 153, 159)
                            background: #fff
                    .time
                        position: absolute
                        top: 0
                        right: 0
                        line-height: 12px
                        font-size: 10px
                        color: rgb(147, 153, 159)
            
                    

                    
                      

            
</style>

