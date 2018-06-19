<template>
    <my-page title="矢量 LOGO 下载">
        <div class="search-box">
            <input class="input" v-model="keyword" placeholder="输入关键词搜索图标，比如：百度">
            <ui-icon-button class="icon" icon="search" @click="search" />
        </div>
        <div class="slogan">10万+ 品牌矢量 LOGO 免费下载</div>
        <div class="loading-box" v-if="loading">
            <div class="ui-loading">
                <ui-circular-progress :size="24"/>
            </div>
        </div>
        <div class="empty" v-if="data && !data.length">搜索不到图标~</div>
        <ul class="logo-list" v-if="data">
            <li class="item" v-for="item in data">
                <div class="svg-box" v-html="item.svg"></div>
                <div class="title">{{ item.title }}
                    <a href="#" @click.prevent="download(item)">下载</a>
                </div>
            </li>

        </ul>
    </my-page>
</template>

<script>
    const saveAs = window.saveAs

    export default {
        data () {
            return {
                loading: false,
                keyword: '',
                data: null,
                page: {
                    menu: [
                        {
                            type: 'icon',
                            icon: 'help',
                            to: '/help'
                        }
                    ]
                }
            }
        },
        methods: {
            download(item) {
                let blob = new Blob([item.svg], {type: 'text/plain;charset=utf-8'})
                saveAs(blob, item.title + '.svg')
            },
            search() {
                if (!this.keyword) {
                    this.$message({
                        type: 'danger',
                        text: '请输入关键词'
                    })
                    return
                }
                this.loading = true
                this.data = null
                this.$http.get('/logos?keyword=' + this.keyword).then(
                    response => {
                        let data = response.data
                        this.data = data
                        console.log(data[0].svg)
                        this.loading = false
                    },
                    response => {
                        console.log(response)
                        this.loading = false
                    })
            }
        }
    }
</script>

<style lang="scss" scoped>
.search-box {
    display: flex;
    width: 400px;
    max-width: 100%;
    margin: 80px auto 16px auto;
    box-shadow: 0 1px 6px rgba(0, 0, 0, 0.117647), 0 1px 4px rgba(0, 0, 0, 0.117647);
    .input {
        flex-grow: 1;
        height: 48px;
        padding-left: 16px;
        outline: none;
        border: none;
    }
}
.slogan {
    text-align: center;
}
.empty {
    margin-top: 80px;
    text-align: center;
    font-size: 18px;
}

</style>
