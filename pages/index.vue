<template>
    <div>
    <v-app-bar
        fixed
        :color="(!scroll)? 'transparent' : layout.header.bgColor"
        :flat="!scroll"
    >
        <v-toolbar-items>
            <v-btn
                text
                class="px-0"
                aria-label="to-top"
                @click="$vuetify.goTo(0)"
            >
                <v-img
                    v-if="layout.header.logo"
                    :src="`${layout.header.logo.url}?fit=clip&h=56`"
                    alt=""
                ></v-img>
              
                <v-toolbar-title
                    v-if="layout.header.title"
                    :style="`color: ${layout.header.txtColor};`"
                >
                    {{ layout.header.title }}
                </v-toolbar-title>
            </v-btn>
        </v-toolbar-items>
      
        <v-spacer></v-spacer>
        
        <v-menu
            v-if="$vuetify.breakpoint.xs"
            right
            nudge-right
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              :color="layout.header.txtColor"
              aria-label="menu"
              v-bind="attrs"
              v-on="on"
              icon
            >
                <v-icon large>mdi-menu</v-icon>
            </v-btn>
          </template>
          <v-list>
            <v-list-item
              v-for="(menu,i) in menus"
                :key="i"
                @click="move(menu.id)"
            >
              <v-list-item-title>{{ menu.title }}</v-list-item-title>
            </v-list-item>
          </v-list>
        </v-menu>
        
        <v-toolbar-items v-else>
            <v-btn
                v-for="(menu,i) in menus"
                :key="i"
                @click="move(menu.id)"
                color="transparent"
                :small="$vuetify.breakpoint.sm"
                :style="`color: ${layout.header.txtColor};`"
                depressed
            >
                <span>{{ menu.title }}</span>
            </v-btn>
        </v-toolbar-items>
        
    </v-app-bar>
    <v-main v-scroll="onScroll">
        <template v-for="(section, i) in sections">
            <template v-if="section.section">
                <div v-if="section.menu" :id="`menu${i}`"></div>
                <Section :section="section.section" />
            </template>
        </template>
    </v-main>
    <v-footer padless :color="layout.footer.bgColor">
        <v-sheet tile width="100%" color="transparent">
            <v-row class="mx-0 justify-center">
                <v-col
                    cols="auto"
                    class="caption"
                    :style="`color: ${layout.footer.txtColor};`"
                >
                    {{ layout.footer.copyright }}
                </v-col>
            </v-row>
        </v-sheet>
    </v-footer>
    </div>
</template>

<script>
import Section from '~/components/section.vue';

export default {
    components: {
      Section  
    },
    async asyncData ({ payload, app }) {
        if(payload) {
            return {
                sections: payload.sections,
                menus: payload.menus,
                layout: payload.layout
            }
        }
        else if(process.env.NODE_ENV !== 'production') {
            var layout = await app.$axios.$get(`https://orienteering.microcms.io/api/v1/layout?depth=3`, {
                headers: { 'X-API-KEY': '6bc5466c-5505-4452-a6a4-b3ecdaaf660f' }
            })
            layout = layout.contents[0]
            var sections = layout.layout
            layout = {
                header: {
                    title: layout.title,
                    logo: layout.logo,
                    bgColor: (layout.bgColorH)? layout.bgColorH : 'blue',
                    txtColor: (layout.txtColorH)? layout.txtColorH : 'white'
                },
                footer: {
                    copyright: layout.copyright,
                    bgColor: (layout.bgColorF)? layout.bgColorF : 'blue',
                    txtColor: (layout.txtColorF)? layout.txtColorF : 'white'
                }
            }
            var menus = sections.map((x, i) => {
                if (x.menu) {
                    return {
                        id: i,
                        title: x.section.title
                    }
                }
            })
            menus = menus.filter(x => x)
            return {
                    sections: sections,
                    menus: menus,
                    layout: layout
            }
        }
    },
    data() {
        return {
            scroll: false,
            name: '',
            email: '',
            content: '' ,
            botField: ''
        }
    },
    methods: {
        onScroll (e) {
            var top = window.pageYOffset ||e.target.scrollTop || 0
            if(top >= window.innerHeight) {
                this.scroll = true
            }
            else {
                this.scroll = false
            }
        },
        move (id) {
            return this.$vuetify.goTo(`#menu${id}`, this.option)
        }
    },
    computed: {
        option () {
            if (this.$vuetify.breakpoint.mdAndUp) {
                return {offset: 64}
            }
            else {
                return {offset: 56}
            }
        }
    }
}
</script>