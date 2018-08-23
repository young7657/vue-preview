<template>
  <div>
    <div class="my-gallery" itemscope itemtype="http://schema.org/ImageGallery">
      <template v-for="item in slides">
        <figure
          itemprop="associatedMedia"
          itemscope
          itemtype="http://schema.org/ImageObject">
          <a :href="item.src" itemprop="contentUrl" :data-size="'' + item.w + 'x' + item.h">
            <img :src="item.msrc" :alt="item.alt" itemprop="thumbnail"/>
          </a>
          <figcaption style="display: none" itemprop="caption description">{{item.title}}</figcaption>
        </figure>
      </template>
    </div>
    <div class="pswp" tabindex="-1" role="dialog" aria-hidden="true">
      <div class="pswp__bg"></div>
      <div class="pswp__scroll-wrap">
        <div class="pswp__container" :class="rotate">
          <div class="pswp__item"></div>
          <div class="pswp__item"></div>
          <div class="pswp__item"></div>
        </div>
        <div class="pswp__ui pswp__ui--hidden">

          <div class="pswp__top-bar">
            <div class="pswp__counter"></div>
            <button class="pswp__button pswp__button--close" title="关闭"></button>
            <button class="pswp__button pswp__button--share" title="Share"></button>
            <button class="pswp__button pswp__button--fs" title="全屏"></button>
            <button class="pswp__button pswp__button--zoom" title="放大/缩小"></button>
            <button class="pswp__button pswp__button--rig" title="向右旋转" @click="TurnRight"></button>
            <button class="pswp__button pswp__button--lef" title="向左旋转" @click="TurnLeft"></button>
            <div class="pswp__preloader">
              <div class="pswp__preloader__icn">
                <div class="pswp__preloader__cut">
                  <div class="pswp__preloader__donut"></div>
                </div>
              </div>
            </div>
          </div>
          <div class="pswp__share-modal pswp__share-modal--hidden pswp__single-tap">
            <div class="pswp__share-tooltip"></div>
          </div>
          <button class="pswp__button pswp__button--arrow--left" title="Previous (arrow left)">
          </button>
          <button class="pswp__button pswp__button--arrow--right" title="Next (arrow right)">
          </button>
          <div class="pswp__caption">
            <div class="pswp__caption__center"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<style>
  @import "~photoswipe/dist/photoswipe.css";
  @import "~photoswipe/dist/default-skin/default-skin.css";

  .pswp__button.pswp__button--rig{background:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAATCAYAAACQjC21AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTM4IDc5LjE1OTgyNCwgMjAxNi8wOS8xNC0wMTowOTowMSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTcgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkVDNzQ5N0RGQTRGMzExRTg5NTI3RjgyQUZDNzBBOUZCIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkVDNzQ5N0UwQTRGMzExRTg5NTI3RjgyQUZDNzBBOUZCIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6RUM3NDk3RERBNEYzMTFFODk1MjdGODJBRkM3MEE5RkIiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6RUM3NDk3REVBNEYzMTFFODk1MjdGODJBRkM3MEE5RkIiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz71fw9FAAABUUlEQVR42qyUzXGDMBCFBUMDtEALuAS7BHzhbpdgSrBLwHcucQmhhNACLVACeY95mgFFlpXEO7Mj0M+3q9XuJvM8G590XZdj2EMLKL8n6FDXdW8CkrhAgS7Qk0CujNAG4MdLIGAVhlagG/QhgNFcIUPcN0APAE9eIGAnwQg5uxudW+y11wg6boDYUOL7izAsHk2E4EyhMyPO7PC/nE+13mohCkaRVwdoCdiVIx8uk3fUaNgKwPD0ekSGqE8V4EkLsdLrkVql1jJHrzNZGkKP4LkugTvFsZLeuZbJ0mj+IIrjTbpIquvm5k2SKkHLdwL5UrmS9f9AxGGQl9ffHmbdq/Y3HlLOqwSNhqlSPn4A5WXDBCXUteqBMeafeswm1G1sg7DpwHybmKMyYvPuoj1HORPsh4XiWWlqWrUv+09jd18xJIGObWt807FVYk+r6luAAQBSaKQdSMkQNAAAAABJRU5ErkJggg==) center center no-repeat}
  .pswp__button.pswp__button--lef{background:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAATCAYAAACQjC21AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTM4IDc5LjE1OTgyNCwgMjAxNi8wOS8xNC0wMTowOTowMSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTcgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkYxMEFDNkQ4QTUwNjExRThBQjA0ODgwNTYyNzcyNkZCIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkYxMEFDNkQ5QTUwNjExRThBQjA0ODgwNTYyNzcyNkZCIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6RjEwQUM2RDZBNTA2MTFFOEFCMDQ4ODA1NjI3NzI2RkIiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6RjEwQUM2RDdBNTA2MTFFOEFCMDQ4ODA1NjI3NzI2RkIiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz6ulMaYAAABXUlEQVR42qxUO26DQBBdr92k5Aor0UU05AjkCK7o8RGgTGmOYMpINN42nWndxRV1uAJtOvImeo5WBAgojDQa7e7Mm8/OzKbrOjVFZVmGEBHY49VNOI7jZkh/MwYIoD1ECg5HfBXgvA/8CxBAEsmZUVU0lKhaqhi+pYz6ANBiEJBgFxqJop0ohegewQk4g24u91vf95O6rk9BEBSQr/T+CIXrVG2h/wl+g80Dji+QV5wbzVRCeDxBSt2exwo+RNDNWBKxV5p1ahi6PHgAv9DBXBJQA5tIw0NLUMUfPbPY1YIof4La8c4STKRdkrJDkp3ZOR6e1P9IMvW0WpfaNQGl3ZpVADnvMgxW97ufE7CUZGIamSw98PCxBBS6KdPN7o3tUs7femcac8CO3Dp2bNsYNnfITVMwndZ5Dwlk3MXw1z5MaOQ5faacc0Ww26wF66yoiBF5BP0e1T7Qnb4EGACd5aKvWQlP6wAAAABJRU5ErkJggg==) center center no-repeat}
  .pswp__rotate_1 .pswp__zoom-wrap .pswp__img{transform: rotate(90deg);}
  .pswp__rotate_2 .pswp__zoom-wrap .pswp__img{transform: rotate(180deg);}
  .pswp__rotate_3 .pswp__zoom-wrap .pswp__img{transform: rotate(270deg);}
  .pswp__rotate_-1 .pswp__zoom-wrap .pswp__img{transform: rotate(-90deg);}
  .pswp__rotate_-2 .pswp__zoom-wrap .pswp__img{transform: rotate(-180deg);}
  .pswp__rotate_-3 .pswp__zoom-wrap .pswp__img{transform: rotate(-270deg);}
</style>
