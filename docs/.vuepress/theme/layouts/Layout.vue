<template>
    <div class="theme-container" >
        <ClientOnly>
            <Navigation></Navigation>
        </ClientOnly>
        <ClientOnly>
            <div class="main_content_wrapper">
                <Home v-if="$page.frontmatter.home"></Home>
                <NewHome v-if="$page.frontmatter.isNewHome"></NewHome>
                <Developer v-if="$page.frontmatter.isDeveloper"></Developer>
                <Community v-if="$page.frontmatter.isCommunity" />
                <div class="md_container" v-if="showMd">
                    <div class="md_wrap">
                        <Content></Content>
                    </div>
                </div>
            </div>
        </ClientOnly>
        <ClientOnly><Footer></Footer></ClientOnly>
    </div>
</template>

<script>
import Navigation from "@theme/components/Navigation.vue";
import Footer from "@theme/components/Footer.vue";
import NewHome from "@theme/components/NewHome.vue";
import Developer from "../components/Developer";
import Community from "../components/Community";
const nav = require("../../config.js");
import cfg from "../../config.json";

export default {
    name: "Layout",
    data() {
        return {
            blogAndArticleList: [],
            articleDetails: null,
            articleName: null,
            articleData: null,
        };
    },
    components: {
        Navigation,
        Footer,
        NewHome,
        Developer,
        Community,
    },
    watch: {
        $route: {
            handler(val, oldval) {
                nav.themeConfig.nav.forEach((item, index) => {
                    if (item.link === val.path) {
                        this.$store.commit("currentIndex", index);
                    }
                });
            },
            immediate: true,
            deep: true,
        },
    },

    mounted() {
        if (localStorage.getItem("currentIndex")) {
            this.$store.commit(
                "currentIndex",
                JSON.parse(localStorage.getItem("currentIndex"))
            );
        }
        // 友盟统计添加
        const script = document.createElement("script");
        script.src = `https://s4.cnzz.com/z_stat.php?id=${cfg.umengId}&web_id=${cfg.umengWebId}`;
        script.language = "JavaScript";
        document.body.appendChild(script);
    },
    computed: {
        showMd() {
            return Object.keys(this.$page.frontmatter).length === 0;
        },
    },
};
</script>
<style lang="stylus">
@import '../styles/adaptation.styl';

.theme-container {
    display: flex;
    flex-direction: column;
    width: 100%;
    // height: 100%;

    .main_content_wrapper {
        margin-top: 6.4rem;

        .md_container {
            font-size: 15px;
            font-weight: 400;
            color: rgba(0, 0, 0, 0.75);
            line-height: 36px;

            .md_wrap {
                padding-top: 60px;
                padding-bottom: 80px;


                @media (max-width: 1200px) {
                    padding-left: 48px;
                    padding-right: 48px;
                }

                @media (max-width: 768px) {
                    padding-left: 24px;
                    padding-right: 24px;
                }

                h1 {
                    line-height: 26px;
                    font-size: 24px;
                    font-weight: 600;
                    color: #000000;
                }

                ul {
                    padding-left: 20px;
                    li::before {
                        content: '';
                        display: inline-block;
                        margin-right: 8px;
                        width: 5px;
                        height: 5px;
                        line-height: 5px;
                        vertical-align: middle;
                        background: #000;
                        border-radius: 50%;
                    }
                }

                .article_about {
                    display: flex;
                    justify-content: space-between;
                    margin-top: 16px;
                    margin-bottom: 15px;
                    line-height: 16px;

                    .article_about_owner_container {
                        display: flex;
                        align-items: center;

                        .article_about_icon_wrap {
                            margin-right: 8px;
                            width: 16px;
                            height: 16px;

                            .article_about_icon {
                                display: inline-block;
                                width: 16px;
                                height: 16px;
                            }
                        }

                        .article_about_owner {
                            height: 16px;
                            font-size: 14px;
                            font-weight: 400;
                            color: rgba(0, 0, 0, 0.75);
                            line-height: 16px;
                        }
                    }

                    .article_about_time {
                        height: 16px;
                        line-height: 16px;
                        font-size: 14px;
                        font-weight: 400;
                        color: rgba(0, 0, 0, 0.64);
                    }
                }

                img {
                    width: 100%;
                    @media (max-width: 375px) {
                        width: 100% !important;
                    }
                }
                .content__default {
                    a {
                        color: #7065FF;
                        span {
                            display: none;
                        }
                    }
                    .language-javascript {
                        border-radius: 4px;
                        &::before {
                            top: 51px;
                            right: 24px;
                        }
                    }
                    ul {
                        li {
                            span {
                                display: none;
                            }
                        }
                    }
                }
            }
        }
    }
}

@media screen and (min-width: $minPcWidth) {
    .theme-container {
        .main_content_wrapper {
            display: flex;
            justify-content: center;

            .md_container {
                width: 960px;

                // display:flex;
                .md_wrap {
                }
            }
        }
    }
}
</style>
