<script setup>
import {ref} from "vue";
import notFound from "@/components/not-found.vue";
import index from '@/components/index.vue'
import community from '@/components/community_classification.vue'
import Show from "@/components/show.vue";
import {MessagePlugin} from "tdesign-vue-next";
import axios from "axios";
import Message from "@/components/message.vue";

let menu_value = ref('1')
const changeHandler = (active) => {
    console.log('change', active);
    menu_value.value = active
};

function redirectToLogin() {
    axios.post('http://127.0.0.1:5000/get_shopping_list', {

    }, {
        headers: {},
        withCredentials: true,
    }).then((data) => {
        if(data['data']['code'] !== 400){
            MessagePlugin.success("你已登录无需重复登录!")
        }
        else{
            window.location.href = 'http://localhost:5173/login';
        }
    });

}

const visible_message = ref(false);
const options = [
    { content: '个人信息', value: 1 },
    { content: '退出', value: 2 },
];
const clickHandler = (data) => {
    MessagePlugin.success(data.content);

};

</script>

<template>
    <t-drawer v-model:visible="visible_message" header="个人信箱" :on-overlay-click="() => (visible_message = false)" placement="right" @cancel="visible_message = false" size="medium">
        <message></message>
    </t-drawer>
    <div class="box">
        <t-head-menu default-value="1" expand-type="popup" @change="changeHandler">
            <template #logo>
                <img style="height: 28px" src="../../public/logo/school-1.jpg" alt="logo" />
                <span style="margin: 5px 0 5px 10px;font-size: 18px">青岛港湾职业技术学院</span>
            </template>
            <t-menu-item value="1"> 学校首页 </t-menu-item>
            <t-submenu value="2" title="学校概况">
                <t-menu-item value="2-1"> 学校社团 </t-menu-item>
                <t-menu-item value="2-2"> 办学理念 </t-menu-item>
                <t-menu-item value="2-3"> 院系介绍 </t-menu-item>
            </t-submenu>
            <t-menu-item value="3"> 专业导读 </t-menu-item>
            <template #operations>
                <t-button variant="text" shape="square" @click="MessagePlugin.success('这个功能还没有写哦')">
                    <template #icon><t-icon name="search" /></template>
                </t-button>
                <t-button variant="text" shape="square"  @click="visible_message = true">
                    <template #icon><t-icon name="mail" /></template>
                </t-button>
                <t-button variant="text" shape="square" @click="redirectToLogin">
                    <template #icon><t-icon name="user" /></template>
                </t-button>
                <t-dropdown :options="options" @click="clickHandler">
                    <t-button  style="border: none" theme="default" variant="outline" shape="square"> <t-icon name="ellipsis" size="16" /></t-button>
                </t-dropdown>
            </template>
        </t-head-menu>
    </div>
    <div class="content_school">
        <div v-if="menu_value === '1'">
            <index></index>
        </div>
        <div v-else-if="menu_value === '2-1'">
            <community></community>
        </div>
        <div v-else-if="menu_value === '2-2'">
            <div style="padding: 20px 0;width: 60%;border-radius: 8px;border: 1px solid rgba(128,128,128,0.4);overflow: hidden;margin: 0 auto">
                <h2  style="width: 100%;margin: 0 auto; text-align: center;font-size: 30px">学院办学理念</h2>
                <div style="width: 100%;margin: 10px auto; text-align: center;font-size: 20px">
                    <h4>校训</h4>
                    <p>以德立身、自强不息；</p>
                    <p>人才培养目标：培养蓝领中的骨干；</p>
                    <p>办学定位：融入海洋战略，服务港航物流，培养优秀人才；</p>
                    <p>学院愿景：建特色名校，育港航人才；</p>
                </div>
            </div>
        </div>
        <div v-else-if="menu_value === '2-3'">
            <show></show>
        </div>
        <div v-else>
            <not-found></not-found>
        </div>
    </div>
</template>
<style lang="less" scoped>
.t-menu__operations {
    .t-button {
        margin-left: 8px;
    }
}
.t-demo-menu--dark {
    .t-button {
        color: #fff;
        &:hover {
            background-color: #4b4b4b;
            border-color: transparent;
            --ripple-color: #383838;
        }
    }
}
</style>
