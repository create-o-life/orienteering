<template>
    <v-img
        :src="(section.bgImg)? section.bgImg.url : undefined"
        :min-height="(section.max)? '100vh' : undefined"
        :height="(section.bgImg)? 'auto' : undefined"
    >
        <v-sheet
            :color="(section.bgColor)? section.bgColor : 'transparent'"
            height="100%"
            tile
        >
            <v-container class="py-10" :style="(section.bgImg)? 'height:100%;' : 'height:100%;min-height:50vh;'">
                <v-row style="height:100%;" class="justify-center align-center">
                    <v-col
                        v-if="section.contents"
                        v-html="section.contents"
                        cols="12"
                        sm="10"
                    ></v-col>
                    <template v-if="section.item">
                        <template v-if="section.slide">
                            <v-col cols="12" sm="8" md="6">
                                <v-carousel
                                    cycle
                                    height="auto"
                                    show-arrows-on-hover
                                    hide-delimiter-background
                                >
                                    <v-carousel-item
                                        v-for="(item,i) in section.item"
                                        :key="i"
                                    >
                                        <v-card
                                            color="transparent"
                                            class="ma-2"
                                            :flat="!item.card"
                                            :tile="!item.card"
                                        >
                                            <v-img :src="(item.bgImg)? item.bgImg.url : undefined" width="100%" height="100%">
                                                <v-sheet
                                                    v-if="item.contents"
                                                    v-html="item.contents"
                                                    :color="(item.bgColor)? item.bgColor : 'transparent'"
                                                    width="100%"
                                                    height="100%"
                                                    class="pa-10"
                                                ></v-sheet>
                                            </v-img>
                                        </v-card>
                                    </v-carousel-item>
                                </v-carousel>
                            </v-col>
                        </template>
                        <template v-else>
                            <template v-for="item in section.item">
                                <v-col
                                    v-if="!item.expand"
                                    cols="12"
                                    :sm="(section.col)? 8 : 6"
                                    :md="(section.col)? 4 : 6"
                                    :class="(item.card)? 'align-self-stretch px-4 px-sm-3' : 'align-self-stretch px-4 px-sm-0 py-0'"
                                >
                                    <template v-if="item.card">
                                        <v-card
                                            color="transparent"
                                            height="100%"
                                        >
                                            <v-img :src="(item.bgImg)? item.bgImg.url : undefined" width="100%" height="100%">
                                                <v-sheet
                                                    v-if="item.contents"
                                                    v-html="item.contents"
                                                    :color="(item.bgColor)? item.bgColor : 'transparent'"
                                                    width="100%"
                                                    height="100%"
                                                    class="pa-5 pa-sm-8"
                                                ></v-sheet>
                                            </v-img>
                                        </v-card>
                                    </template>
                                    <template v-else>
                                        <v-img :src="(item.bgImg)? item.bgImg.url : undefined" width="100%" height="100%">
                                            <v-sheet
                                                :color="(item.bgColor)? item.bgColor : 'transparent'"
                                                height="100%"
                                                width="100%"
                                                class="d-flex"
                                                tile
                                            >
                                                <v-card
                                                    v-if="item.contents"
                                                    v-html="item.contents"
                                                    color="transparent"
                                                    class="ma-auto pa-10"
                                                    flat
                                                    tile
                                                ></v-card>
                                            </v-sheet>
                                        </v-img>
                                    </template>
                                </v-col>
                                <v-col
                                    v-else
                                    cols="12"
                                    sm="10"
                                    :class="(item.card)? 'align-self-stretch px-4 px-sm-3' : 'align-self-stretch px-4 px-sm-0 py-0'"
                                >
                                    <template v-if="item.card">
                                        <v-card
                                            color="transparent"
                                            height="100%"
                                        >
                                            <v-img :src="(item.bgImg)? item.bgImg.url : undefined" width="100%" height="100%">
                                                <v-sheet
                                                    v-if="item.contents"
                                                    v-html="item.contents"
                                                    :color="(item.bgColor)? item.bgColor : 'transparent'"
                                                    width="100%"
                                                    height="100%"
                                                    class="pa-10"
                                                ></v-sheet>
                                            </v-img>
                                        </v-card>
                                    </template>
                                    <template v-else>
                                        <v-img :src="(item.bgImg)? item.bgImg.url : undefined" width="100%" height="100%">
                                            <v-sheet
                                                :color="(item.bgColor)? item.bgColor : 'transparent'"
                                                height="100%"
                                                width="100%"
                                                class="d-flex"
                                                tile
                                            >
                                                <v-card
                                                    v-if="item.contents"
                                                    v-html="item.contents"
                                                    color="transparent"
                                                    class="ma-auto pa-10"
                                                    flat
                                                    tile
                                                ></v-card>
                                            </v-sheet>
                                        </v-img>
                                    </template>
                                </v-col>
                            </template>
                        </template>
                    </template>
                    <v-col v-if="section.movie" cols="12" sm="8" md="6">
                        <div style="width: 100%;padding-bottom: 56.25%;height: 0px;position: relative;">
                            <iframe
                                style="position: absolute;top: 50%;left: 50%;transform: translate(-50%, -50%);-webkit-transform: translate(-50%, -50%);-ms-transform: translate(-50%, -50%);"
                                width="100%"
                                height="100%"
                                title="Movie"
                                :src="`https://www.youtube.com/embed/${section.movie}`"
                                frameborder="0"
                                allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
                                allowfullscreen
                            ></iframe>
                        </div>
                    </v-col>
                    <v-col
                        v-if="section.btnLabel&&section.btnLink"
                        cols="12"
                        sm="10"
                        class="d-flex justify-center"
                    >
                        <v-btn
                            :to="section.btnLink"
                        >
                            {{ section.btnLabel }}
                        </v-btn>
                    </v-col>
                    <v-col
                        v-if="section.contact"
                        cols="12"
                        sm="8"
                        md="6"
                    >
                        <v-form name="contact" method="POST" data-netlify="true" netlify-honeypot="bot-field">
                            <input type="hidden" name="form-name" value="contact">
                            <v-text-field
                                v-model="name"
                                label="名前"
                                name="name"
                            ></v-text-field>
                            <v-text-field
                                v-model="email"
                                label="メールアドレス"
                                name="email"
                            ></v-text-field>
                            <v-textarea
                                v-model="content"
                                label="内容"
                                name="content"
                                outlined
                            ></v-textarea>
                            <v-text-field
                                v-model="botField"
                                label="人間は入力しないでください"
                                name="bot-field"
                                v-show="false"
                            />
                            <v-btn
                                type="submit"
                                color="postTitleBg"
                                class="postTitleTxt--text"
                            >
                                送信
                            </v-btn>
                        </v-form>
                    </v-col>
                </v-row>
            </v-container>
        </v-sheet>
    </v-img>
</template>

<script>

export default {

    data() {
        return {
            name: '',
            email: '',
            content: '' ,
            botField: ''
        }
    },
    props: ['section']
}
</script>