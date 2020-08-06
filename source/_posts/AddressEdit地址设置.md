---
layout: post
title: "Vant 地址编辑"
date: 2020-7-20 15:01:54
comments: true
tags: 
	- Vant
---

在vue中使用vant的`AddressEdit`组件,因为是ui库封好的组件,添加修改复用一个页面导致修改时有默认值,需要绑定在AddressEdit组件上

<!-- more -->

```javascript
<template>
  <div class="addressRedact">
    <van-address-edit
      :address-info="addressInfo"
      :area-list="areaList"
      show-set-default
      save-button-text="完成"
      :area-columns-placeholder="['请选择', '请选择', '请选择']"
      @save="onSave"
    />
  </div>
</template>
<script>
import cityData from "../../assets/js/cityData";
export default {
  data() {
    return {
      areaList: {
        province_list: cityData.province_list,
        city_list: cityData.city_list,
        county_list: cityData.county_list
      },
      addressInfo: {
        //收货人信息初始值
        name: "", //姓名
        tel: "", //电话
        province: "", //省份
        city: "", //城市
        country: "", //区县
        areaCode: "", //地址code：ID
        addressDetail: "", //详细地址
        isDefault: false //是否选择默认
      }
    };
  },
  mounted() {
  // 这里判断是否是编辑跳转过的
    if (this.$route.query.linkman) {
      this.addressInfo.name = this.$route.query.linkman;
      this.addressInfo.tel = this.$route.query.phone;
      this.addressInfo.addressDetail = this.$route.query.address_detail;
      let address = this.$route.query.address.split("/");
      // 地区的回显
      if (address.length > 3) {
        for (let item in this.areaList.county_list) {
          if (this.areaList.county_list[item] == address[2]) {
            this.addressInfo.areaCode = item;
          }
        }
      }
    }
  },
  methods: {
    onSave(data) {
      console.info(data);
    }
  }
};
</script>
```

特别提示: `addressInfo `在`data`里声明事需要把 需要他参数提前声明在里边, 如果是空对象时,在其他地方赋值会导致没有绑定到组件上



`areaCode ` 需要的是县的id 

cityData 是引用  Vant的[Area.json](https://github.com/youzan/vant/blob/dev/src/area/demo/area.js)