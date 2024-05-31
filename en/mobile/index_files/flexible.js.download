
function IeVersion() {
  //Set defaults
  var value = {
    IsIE: false,
    IsEdge: false,
    EdgeHtmlVersion: 0,
    TrueVersion: 0,
    ActingVersion: 0,
    CompatibilityMode: false
  };

  //Try to find the Trident version number
  var trident = navigator.userAgent.match(/Trident\/(\d+)/);
  if (trident) {
    value.IsIE = true;
    //Convert from the Trident version number to the IE version number
    value.TrueVersion = parseInt(trident[1], 10) + 4;
  }

  //Try to find the MSIE number
  var msie = navigator.userAgent.match(/MSIE (\d+)/);
  if (msie) {
    value.IsIE = true;
    //Find the IE version number from the user agent string
    value.ActingVersion = parseInt(msie[1]);
  } else {
    //Must be IE 11 in "edge" mode
    value.ActingVersion = value.TrueVersion;
  }

  //If we have both a Trident and MSIE version number, see if they're different
  if (value.IsIE && value.TrueVersion > 0 && value.ActingVersion > 0) {
    //In compatibility mode if the trident number doesn't match up with the MSIE number
    value.CompatibilityMode = value.TrueVersion != value.ActingVersion;
  }

  //Try to find Edge and the EdgeHTML vesion number
  var edge = navigator.userAgent.match(/Edge\/(\d+\.\d+)$/);
  if (edge)
  {
    value.IsEdge = true;
    value.EdgeHtmlVersion = edge[1];
  }

  return value;
}
function insertScript(src) {
  var script = document.createElement('script');
  script.async = "async";
  script.src = src;
  document.getElementsByTagName("head")[0].appendChild( script );
}
function getTemplate() {
  var lang = navigator.language || navigator.userLanguage;//常规浏览器语言和IE浏览器
  lang = lang.substr(0, 2); // 截取lang前2位
  var langData = lang.toLowerCase() !== 'zh' ? {
    title: 'You seem to be using an unspported browser',
    content: 'To get the most out of using the new Return Path please use new experience with a  supported browser.'
  } : {
    title: '哦，你的浏览器暂时无法完全支持',
    content: '您正在使用的浏览器版本过低，建议您使用以下可支持的浏览器版本。'
  };
  var tem =  '<div class="badbrowser__helper"></div>\n      <div class="badbrowser__content">\n        <div class="unsuport-title">$title</div>\n        <div class="unsuport-content">$content</div>\n        <p>\n          <a class="oldbrowser__browserLink" title="Download Internet Explorer" href="https://www.microsoft.com/en-us/download/internet-explorer-11-for-windows-7-details.aspx" target="_blank">\n            <img width="72" height="70" src="/alerts/images/ie11.png">\n            <span>IE 11</span>\n          </a>\n          <a class="oldbrowser__browserLink" title="Download Google Chrome" href="https://www.google.com/chrome/" target="_blank">\n            <img width="70" height="70" src="/alerts/images/chrome.png">\n            <span>Chrome</span>\n          </a>\n          <a class="oldbrowser__browserLink" title="Download Mozilla Firefox" href="https://www.mozilla.org" target="_blank">\n            <img width="71" height="70" src="/alerts/images/firefox.png">\n            <span>Firefox</span>\n          </a>\n          <a class="oldbrowser__browserLink" title="Download Safari" href="https://www.apple.com/safari/" target="_blank">\n            <img width="71" height="70" src="/alerts/images/safari.png">\n            <span>Safari</span>\n          </a>\n          <a class="oldbrowser__browserLink" title="Download Opera" href="http://www.opera.com/download" target="_blank">\n            <img width="66" height="70" src="/alerts/images/opera.png">\n            <span>Opera</span>\n          </a>\n        </p>';
  return tem.replace('$title',langData.title).replace('$content', langData.content);
}
var ieVersion = new IeVersion();
if (ieVersion.IsIE) {
  if (ieVersion.TrueVersion < 9) {
    window.onload = function () {
      var el  = document.createElement('div');
      el.className = 'badbrowser';
      el.innerHTML = getTemplate();
      document.body.appendChild(el);
    }
  } else if(ieVersion.TrueVersion <= 11){
    insertScript('/js/polyfill.min.js');
  }
} else {
  // alert('not ie');
}

if (typeof console !== 'undefined') {
  console.log && (console.log = function () {});
  console.error && (console.error = function () {});
  console.warn && (console.warn = function () {});
}
(function flexible (window, document) {
  var docEl = document.documentElement;
  var dpr = window.devicePixelRatio || 1;

  // adjust body font size
  function setBodyFontSize () {
    if (document.body) {
      document.body.style.fontSize = (12 * dpr) + 'px'
    }
    else {
      document.addEventListener('DOMContentLoaded', setBodyFontSize)
    }
  }
  setBodyFontSize();

  // set 1rem = viewWidth / 10
  function setRemUnit () {
    var clientWidth = docEl.clientWidth;
    var maxWidth = location.href.indexOf('download3') > 0 ? 750: 1440;
    if (clientWidth >= maxWidth) {
      clientWidth = maxWidth;
    }
    var rem = clientWidth / 10;
    docEl.style.fontSize = rem + 'px'
  }

  setRemUnit();

  // reset rem unit on page resize
  window.addEventListener('resize', setRemUnit);
  window.addEventListener('pageshow', function (e) {
    if (e.persisted) {
      setRemUnit()
    }
  });

  // detect 0.5px supports
  if (dpr >= 2) {
    var fakeBody = document.createElement('body');
    var testElement = document.createElement('div');
    testElement.style.border = '.5px solid transparent';
    fakeBody.appendChild(testElement);
    docEl.appendChild(fakeBody);
    if (testElement.offsetHeight === 1) {
      docEl.classList.add('hairlines')
    }
    docEl.removeChild(fakeBody);
  }
}(window, document));

  window.addEventListener('load',function () {
    document.addEventListener('click', function (e) {
     var eventName = e.target.getAttribute('data-event') || e.target.dataset['event'];
     console.log('event', eventName);
      if (eventName) {
        sa.trackLink(e.target, eventName, {});
      }
    });
  });
