<template>
  <div>
    <v-navigation-drawer
      v-model="drawer"
      :clipped="$vuetify.breakpoint.lgAndUp"
      app>
      <v-list dense>
        <template v-for="item in items">
          <v-layout v-if="item.heading" :key="item.heading" align-center>
            <v-flex xs6>
              <v-subheader v-if="item.heading">{{ item.heading }}</v-subheader>
            </v-flex>
            <v-flex xs6 class="text-center">
              <a href="#!" class="body-2 black--text">EDIT</a>
            </v-flex>
          </v-layout>
          <v-list-group
            v-else-if="item.children"
            :key="item.text"
            v-model="item.model"
            :prepend-icon="item.model ? item.icon : item['icon-alt']"
            append-icon
          >
            <template v-slot:activator>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>{{ item.text }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </template>
            <v-list-item
              v-for="(child, i) in item.children"
              :key="i" 
              @click="clickMenu(child.id)">
              <v-list-item-action v-if="child.icon">
                <v-icon>{{ child.icon }}</v-icon>
              </v-list-item-action>
              <v-list-item-content>
                <v-list-item-title>{{ child.text }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-group>
          <v-list-item v-else :key="item.text" @click="clickMenu(item.id)">
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>{{ item.text }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </template>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      :clipped-left="$vuetify.breakpoint.lgAndUp"
      app
      color="blue darken-3"
      dark
      >
      <v-toolbar-title style="width: 300px" class="ml-0 pl-4">
        <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
        <span class="hidden-sm-and-down">药局管理</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-tooltip right>
        <template v-slot:activator="{ on }">
          <v-btn icon to="/about" v-on="on">
            <v-icon>thumb_up_alt</v-icon>
          </v-btn>
        </template>
        <span>帮助手册</span>
      </v-tooltip>
      <v-btn icon to="/">
        <v-icon>notifications</v-icon>
      </v-btn>
      <v-btn icon large @click="selectSource">
        <v-avatar size="32px" item>
          <v-img :src="require('../assets/logo.svg')" alt="苜蓿草科技"></v-img>
        </v-avatar>
      </v-btn>
    </v-app-bar>
    <!-- <v-container fluid fill-height>
      <v-layout justify-start>
        <v-img :src="require('../assets/main_bg.jpg')" />
      </v-layout>
    </v-container>-->
    <v-tooltip right>
      <template v-slot:activator="{ on }">
        <v-btn
          bottom
          color="pink"
          dark
          fab
          fixed
          right 
          @click="clickMenu('logout')"
          v-on="on"
          ><v-icon>add</v-icon>
        </v-btn>
      </template>
      <span>退出登录</span>
    </v-tooltip>
  </div>
</template>

<script>
export default {
  props: {
    source: String
  },
  data: () => ({
    dialog: false,
    drawer: null,
    items: [
      { icon: "contacts", text: "药品处方发药", id: "despensing" },
      { icon: "history", text: "药品科室发药", id: "despensing_dept" },
      { icon: "content_copy", text: "药品申领", id: "drug_taking" },
      { icon: "keyboard", text: "协定药品申领", id: "drug_taking2" },
      { icon: "keyboard", text: "药品库存", id: "drug_stock" },
      { icon: "keyboard", text: "交接班盘点", id: "drug_c" },
      { icon: "keyboard", text: "大处方管理", id: "drug_d" },
      { icon: "keyboard", text: "药局工作量分析", id: "drug_e" },
      { icon: "keyboard", text: "药品月结", id: "drug_f" },      
      {
        icon: "settings",
        "icon-alt": "keyboard_arrow_down",
        text: "管理与维护",
        model: false,
        children: [
          { text: "月结参数设置", id: "mg_dict" },
          { text: "药局设置", id: "mg_analyse" },
          { text: "操作员设置", id: "mg_analyse1" }
        ]
      },
      { icon: "help", text: "退出登录", id: "logout" }
    ]
  }),
  methods: {
    clickMenu(tstr) {
      console.log("点击=" + tstr);
      if (tstr === "logout") {
        localStorage.removeItem("user");
        this.$router.push({ path: "/login" });
      }
      switch (tstr)
      {
        case "out_reg":
          this.$router.push({ path: "/outreg" });
          break;
        case "out_cash":
          this.$router.push({ path: "/outcash" });
          break;
        case "out_chk":
          this.$router.push({ path: "/outchk" });
          break;
        case "out_receipt":
          this.$router.push({ path: "/outreceipt" });
          break;
        case "detail_reg":
          this.$router.push({ path: "/detailreg" });
          break;
        case "detail_cash":
          this.$router.push({ path: "/detailcash" });
          break;
        case "detail_chk":
          this.$router.push({ path: "/detailchk" });
          break;
        case "detail_undo":
          this.$router.push({ path: "/detailundo" });
          break;
        case "detail_op":
          this.$router.push({ path: "/detailop" });
          break;
        case "mg_dict":
          this.$router.push({ path: "/mgdict" });
          break;
        case "mg_analyse":
          this.$router.push({ path: "/mganalyse" });
          break;
        case "mg_invoice":
          this.$router.push({ path: "/mginvoice" });
          break;
        default:
          localStorage.removeItem("user");
          this.$router.push({ path: "/login" });
      }
    },

    selectSource() {
      window.location.href = "http://www.cloveropen.com";
    }
  }
};
</script>
