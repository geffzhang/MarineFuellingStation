﻿<template>
    <div id="root">
        <yd-tab :callback="change">
    
            <yd-tab-panel label="所有分类">
                <yd-cell-group>
                    <yd-cell-item arrow @click.native="stClick(st)" v-for="st in sts" :key="st.id">
                        <div slot="left" class="lineheight24" style="margin: 10px 0 10px 0">
                            <p>【{{st.name}}】- {{st.stores.length}}个仓</p>
                            <p class="col-light-gray font14">总容量：{{sumVolume(st)}}升</p>
                        </div>
                        <div slot="right" class="align-right" style="margin: 10px 0 10px 0">
                            <p class="col-green font16">{{sumValue(st)}}升</p>
                            <!--<p style="color:lightcoral; font-size: 16px">￥{{sumCost(st)}}</p>-->
                        </div>
                    </yd-cell-item>
                </yd-cell-group>
            </yd-tab-panel>
            <yd-tab-panel :label="isAddStore?'添加':'编辑'" :active="isAddStore?false:true">
                <yd-cell-group>
                    <yd-cell-item>
                        <yd-radio-group v-model="radio1" slot="left">
                            <yd-radio val="0">销售仓</yd-radio>
                            <yd-radio val="1">存储仓</yd-radio>
                        </yd-radio-group>
                    </yd-cell-item>
                    <yd-cell-item>
                        <yd-radio-group v-model="model.isForLand" slot="left">
                            <yd-radio val="false">水上</yd-radio>
                            <yd-radio val="true">陆上</yd-radio>
                        </yd-radio-group>
                    </yd-cell-item>
                    <yd-cell-item>
                        <span slot="left">所属分类：</span>
                        <select slot="right" v-model="model.storeTypeId">
                            <option value="-1">请选择分类</option>
                            <option v-for="st in sts" :key="st.id" :value="st.id">{{st.name}}</option>
                        </select>
                        <span slot="right">
                            <yd-button type="warning" @click.native="newstShowClick">添加</yd-button>
                        </span>
                    </yd-cell-item>
                    <yd-cell-item>
                        <span slot="left">名称：</span>
                        <yd-input slot="right" v-model="model.name" required placeholder="请输入名称"></yd-input>
                    </yd-cell-item>
                    <yd-cell-item>
                        <span slot="left">容量：</span>
                        <yd-input type="number" slot="right" v-model="model.volume" required placeholder="请输入容量"></yd-input>
                        <span slot="right">升</span>
                    </yd-cell-item>
                </yd-cell-group>
                <div class="align-center">
                    <yd-button v-show="isAddStore" size="large" type="primary" @click.native="addStoreclick">提交</yd-button>
                    <yd-button v-show="!isAddStore" size="large" type="primary" @click.native="saveStoreclick">保存</yd-button>
                </div>
            </yd-tab-panel>
        </yd-tab>
        <yd-popup v-model="stshow" position="right" width="70%">
            <yd-cell-group :title="'【' + currentst.name + '】'">
                <yd-cell-item arrow v-for="s in currentst.stores" :key="s.id" @click.native="editStoreclick(s)">
                    <div slot="left">{{s.name}}</div>
                    <div slot="right">
                        <p class="col-green font14">{{s.value}} / {{s.volume}}升</p>
                    </div>
                </yd-cell-item>
            </yd-cell-group>
            <div class="align-center">
                <yd-button size="large" type="primary" @click.native="editStoreTypeclick">编辑分类</yd-button>
            </div>
        </yd-popup>
        <yd-popup v-model="newstshow" position="right" width="70%">
            <yd-cell-group :title="isAddStoreType?'添加分类':'编辑分类'">
                <yd-cell-item>
                    <span slot="left">分类名称：</span>
                    <yd-input slot="right" v-model="stName" required placeholder="请输入"></yd-input>
                </yd-cell-item>
            </yd-cell-group>
            <div class="align-center">
                <yd-button v-show="isAddStoreType" size="large" type="primary" @click.native="postStoreType">提交</yd-button>
                <yd-button v-show="!isAddStoreType" size="large" type="primary" @click.native="saveStoreTypeclick">保存</yd-button>
            </div>
        </yd-popup>
    </div>
</template>

<script src="./store.ts" />

