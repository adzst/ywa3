
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>{{$t('workbench')}}</title>
    <link rel="stylesheet" href="./style.css" />
    <link rel="stylesheet" href="./quasar.prod.css" />
    <script src="./js/moment.min.js"></script>
    <script src="./js/rpa.js"></script>
  </head>

  <body>
    <div id="app" style="display: none">
      <div class="open-info">
        <div class="box q-pb-sm">
          <div class="open-ip">
            <div class="chuhai2345 bg-white flex flex-center q-pa-sm q-mt-xs">
              <a href="http://chuhai2345.com" class="text-bold" target="_blank">{{$t('chuhai2345')}}</a>
            </div>
            <div class="ip-box">
              <div class="ip">
                <div class="ip-ip flex flex-center">
                  <div class="ip-result">{{ipData.ip || $t('connecting')}}</div>
                </div>
                <div class="tip q-mt-sm">
                  <q-btn
                    :label="$t('agentAgain')"
                    color="info"
                    unelevated
                    size="sm"
                    no-caps
                    @click="confirm = true"
                    class="q-ml-sm"
                    v-if="browser.proxyMethod === 3"
                  >
                    <span
                      style="
                        background: #fff;
                        width: 16px;
                        height: 16px;
                        border-radius: 20px;
                        color: black;
                        font-size: 12px;
                        margin-left: 6px;
                      "
                      >i</span
                    >
                    <q-tooltip>{{$t('agentTips')}}</q-tooltip>
                  </q-btn>
                  <q-btn
                    :label="$t('refreshIp')"
                    color="info"
                    unelevated
                    size="sm"
                    no-caps
                    @click="refreshAgent"
                    class="q-ml-sm"
                    v-if="browser.proxyType !== 'noproxy'"
                  >
                    <span
                      style="
                        background: #fff;
                        width: 16px;
                        height: 16px;
                        border-radius: 20px;
                        color: black;
                        font-size: 12px;
                        margin-left: 6px;
                      "
                      >i</span
                    >
                    <q-tooltip>{{$t('refreshIpTips')}}</q-tooltip>
                  </q-btn>
                  <q-btn
                    v-if="browser.proxyMethod === 2 && browser.proxyType !== 'noproxy'"
                    :label="$t('refreshProxy')"
                    color="info"
                    unelevated
                    size="sm"
                    no-caps
                    @click="refreshProxy"
                    class="q-ml-sm"
                    :loading="refreshProxyLoading"
                  >
                    <span
                      style="
                        background: #fff;
                        width: 16px;
                        height: 16px;
                        border-radius: 20px;
                        color: black;
                        font-size: 12px;
                        margin-left: 6px;
                      "
                      >i</span
                    >
                    <q-tooltip style="width: 400px">{{$t('refreshProxyTips')}}</q-tooltip>
                  </q-btn>
                </div>
                <div class="flex flex-center q-mt-md ip-infos" style="line-height: 30px; color: #00fffe">
                  <div style="border: 1px dashed rgba(255, 255, 255, 0.4); border-radius: 6px" class="q-pl-sm q-pr-md text-bold">
                    <span>{{$t('recommend')}}</span>
                    <span> {{ipData.countryName}} </span> /
                    <span> {{ipData.stateProv}} </span>
                    /
                    <span> {{ipData.city}} </span>
                  </div>
                </div>
                <div class="flex flex-center q-mt-sm ip-infos">
                  <div
                    style="border: 1px dashed rgba(255, 255, 255, 0.4); border-radius: 6px; line-height: 30px"
                    class="q-pl-md q-pr-md text-bold"
                  >
                    <span>{{$t('timeZone')}}:{{ipData.timeZone}}</span>
                    <span class="q-ml-md">{{$t('longlat')}}:{{ipData.longitude}}/{{ipData.latitude}}</span>
                    <span class="q-ml-md">{{$t('zip')}}:{{ipData.zip}}</span>
                  </div>
                </div>
                <ul class="ping q-mt-md">
                  <li v-for="item of urls" :key="item.url" :class="{success: item.success}" @click="openUrl(item.url)">{{item.name}}</li>
                </ul>
              </div>
            </div>
          </div>
          <div class="proxy text-center" v-show="!ipData.ip">{{$t('proxyTips')}}</div>
          <div class="content" style="text-align: left">
            <div class="row-left">
              <div class="bd">{{$t('seq')}}</div>
              <div class="hd row items-center">
                {{browser.seq }} <span class="q-ml-sm">（{{$t('launchTime')}}: {{browser.operTime}}）</span>
              </div>
            </div>
            <div class="row-left">
              <div class="bd">{{$t('username')}}:</div>
              <div class="hd">
                {{browser.userName || $t('noUsername')}}<span class="q-ml-xs q-mr-xs">/</span>{{browser.password || $t('noPass')}}
              </div>
            </div>
            <div class="row-left">
              <div class="bd">{{$t('group')}}:</div>
              <div class="hd">{{browser.groupName || $t('noGroup')}}</div>
            </div>
            <div class="row-left">
              <div class="bd">{{$t('name')}}:</div>
              <div class="hd">{{browser.name || $t('noName')}}</div>
            </div>
            <div class="row-left">
              <div class="bd">{{$t('remark')}}:</div>
              <div class="hd">{{browser.remark || $t('noRemark')}}</div>
            </div>
          </div>
          <div class="footer q-pa-sm">
            <div class="row q-col-gutter-x-sm">
              <div class="col-12 col-sm-6" v-for="item of ads" :key="item.id">
                <a :href="item.comments" target="_blank">
                  <img :src="item.dicName" alt="" />
                </a>
              </div>
            </div>
          </div>
        </div>
        <div class="version">
          Copyright <i class="ri-copyright-line" aria-hidden="true" data-v-4bc6556a="" data-v-cff1b8ee=""></i> {{$t('bitBrowser')}} 2023
        </div>
      </div>
      <q-dialog v-model="confirm" persistent>
        <q-card style="width: 400px">
          <q-card-section class="row items-center">
            <span>{{$t('reProxy')}}</span>
          </q-card-section>

          <q-card-actions align="right">
            <q-btn unelevated style="background: #ececec" class="q-mr-sm" v-close-popup size="small">{{$t('cancel')}}</q-btn>
            <q-btn color="primary" unelevated @click="againAgent" :loading="loading" size="small">{{$t('confirm')}}</q-btn>
          </q-card-actions>
        </q-card>
      </q-dialog>
    </div>
    <script src="./js/vue.global.prod.js"></script>
    <script src="./js/vue-i18n.global.js"></script>
    <script src="./js/quasar.umd.prod.js"></script>
    <script src="./js/index.js"></script>
  </body>
</html>

