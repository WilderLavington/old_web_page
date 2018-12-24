---
layout: post
title: "Ongoing Machine Learning Notes: Variance Reduction and the Generalized Advantage Estimator"
date: 2018-12-30
---

<head><meta charset="utf-8" />
<title>Maximum_Entropy_Policy_Gradients</title><script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.1.10/require.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.0.3/jquery.min.js"></script>

<style type="text/css">
    /*!
*
* Twitter Bootstrap
*
*/
/*!
 * Bootstrap v3.3.7 (http://getbootstrap.com)
 * Copyright 2011-2016 Twitter, Inc.
 * Licensed under MIT (https://github.com/twbs/bootstrap/blob/master/LICENSE)
 */
/*! normalize.css v3.0.3 | MIT License | github.com/necolas/normalize.css */
html {
  font-family: sans-serif;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
}
body {
  margin: 0;
}
article,
aside,
details,
figcaption,
figure,
footer,
header,
hgroup,
main,
menu,
nav,
section,
summary {
  display: block;
}
audio,
canvas,
progress,
video {
  display: inline-block;
  vertical-align: baseline;
}
audio:not([controls]) {
  display: none;
  height: 0;
}
[hidden],
template {
  display: none;
}
a {
  background-color: transparent;
}
a:active,
a:hover {
  outline: 0;
}
abbr[title] {
  border-bottom: 1px dotted;
}
b,
strong {
  font-weight: bold;
}
dfn {
  font-style: italic;
}
h1 {
  font-size: 2em;
  margin: 0.67em 0;
}
mark {
  background: #ff0;
  color: #000;
}
small {
  font-size: 80%;
}
sub,
sup {
  font-size: 75%;
  line-height: 0;
  position: relative;
  vertical-align: baseline;
}
sup {
  top: -0.5em;
}
sub {
  bottom: -0.25em;
}
img {
  border: 0;
}
svg:not(:root) {
  overflow: hidden;
}
figure {
  margin: 1em 40px;
}
hr {
  box-sizing: content-box;
  height: 0;
}
pre {
  overflow: auto;
}
code,
kbd,
pre,
samp {
  font-family: monospace, monospace;
  font-size: 1em;
}
button,
input,
optgroup,
select,
textarea {
  color: inherit;
  font: inherit;
  margin: 0;
}
button {
  overflow: visible;
}
button,
select {
  text-transform: none;
}
button,
html input[type="button"],
input[type="reset"],
input[type="submit"] {
  -webkit-appearance: button;
  cursor: pointer;
}
button[disabled],
html input[disabled] {
  cursor: default;
}
button::-moz-focus-inner,
input::-moz-focus-inner {
  border: 0;
  padding: 0;
}
input {
  line-height: normal;
}
input[type="checkbox"],
input[type="radio"] {
  box-sizing: border-box;
  padding: 0;
}
input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  height: auto;
}
input[type="search"] {
  -webkit-appearance: textfield;
  box-sizing: content-box;
}
input[type="search"]::-webkit-search-cancel-button,
input[type="search"]::-webkit-search-decoration {
  -webkit-appearance: none;
}
fieldset {
  border: 1px solid #c0c0c0;
  margin: 0 2px;
  padding: 0.35em 0.625em 0.75em;
}
legend {
  border: 0;
  padding: 0;
}
textarea {
  overflow: auto;
}
optgroup {
  font-weight: bold;
}
table {
  border-collapse: collapse;
  border-spacing: 0;
}
td,
th {
  padding: 0;
}
/*! Source: https://github.com/h5bp/html5-boilerplate/blob/master/src/css/main.css */
@media print {
  *,
  *:before,
  *:after {
    background: transparent !important;
    color: #000 !important;
    box-shadow: none !important;
    text-shadow: none !important;
  }
  a,
  a:visited {
    text-decoration: underline;
  }
  a[href]:after {
    content: " (" attr(href) ")";
  }
  abbr[title]:after {
    content: " (" attr(title) ")";
  }
  a[href^="#"]:after,
  a[href^="javascript:"]:after {
    content: "";
  }
  pre,
  blockquote {
    border: 1px solid #999;
    page-break-inside: avoid;
  }
  thead {
    display: table-header-group;
  }
  tr,
  img {
    page-break-inside: avoid;
  }
  img {
    max-width: 100% !important;
  }
  p,
  h2,
  h3 {
    orphans: 3;
    widows: 3;
  }
  h2,
  h3 {
    page-break-after: avoid;
  }
  .navbar {
    display: none;
  }
  .btn > .caret,
  .dropup > .btn > .caret {
    border-top-color: #000 !important;
  }
  .label {
    border: 1px solid #000;
  }
  .table {
    border-collapse: collapse !important;
  }
  .table td,
  .table th {
    background-color: #fff !important;
  }
  .table-bordered th,
  .table-bordered td {
    border: 1px solid #ddd !important;
  }
}
@font-face {
  font-family: 'Glyphicons Halflings';
  src: url('../components/bootstrap/fonts/glyphicons-halflings-regular.eot');
  src: url('../components/bootstrap/fonts/glyphicons-halflings-regular.eot?#iefix') format('embedded-opentype'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.woff2') format('woff2'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.woff') format('woff'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.ttf') format('truetype'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.svg#glyphicons_halflingsregular') format('svg');
}
.glyphicon {
  position: relative;
  top: 1px;
  display: inline-block;
  font-family: 'Glyphicons Halflings';
  font-style: normal;
  font-weight: normal;
  line-height: 1;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.glyphicon-asterisk:before {
  content: "\002a";
}
.glyphicon-plus:before {
  content: "\002b";
}
.glyphicon-euro:before,
.glyphicon-eur:before {
  content: "\20ac";
}
.glyphicon-minus:before {
  content: "\2212";
}
.glyphicon-cloud:before {
  content: "\2601";
}
.glyphicon-envelope:before {
  content: "\2709";
}
.glyphicon-pencil:before {
  content: "\270f";
}
.glyphicon-glass:before {
  content: "\e001";
}
.glyphicon-music:before {
  content: "\e002";
}
.glyphicon-search:before {
  content: "\e003";
}
.glyphicon-heart:before {
  content: "\e005";
}
.glyphicon-star:before {
  content: "\e006";
}
.glyphicon-star-empty:before {
  content: "\e007";
}
.glyphicon-user:before {
  content: "\e008";
}
.glyphicon-film:before {
  content: "\e009";
}
.glyphicon-th-large:before {
  content: "\e010";
}
.glyphicon-th:before {
  content: "\e011";
}
.glyphicon-th-list:before {
  content: "\e012";
}
.glyphicon-ok:before {
  content: "\e013";
}
.glyphicon-remove:before {
  content: "\e014";
}
.glyphicon-zoom-in:before {
  content: "\e015";
}
.glyphicon-zoom-out:before {
  content: "\e016";
}
.glyphicon-off:before {
  content: "\e017";
}
.glyphicon-signal:before {
  content: "\e018";
}
.glyphicon-cog:before {
  content: "\e019";
}
.glyphicon-trash:before {
  content: "\e020";
}
.glyphicon-home:before {
  content: "\e021";
}
.glyphicon-file:before {
  content: "\e022";
}
.glyphicon-time:before {
  content: "\e023";
}
.glyphicon-road:before {
  content: "\e024";
}
.glyphicon-download-alt:before {
  content: "\e025";
}
.glyphicon-download:before {
  content: "\e026";
}
.glyphicon-upload:before {
  content: "\e027";
}
.glyphicon-inbox:before {
  content: "\e028";
}
.glyphicon-play-circle:before {
  content: "\e029";
}
.glyphicon-repeat:before {
  content: "\e030";
}
.glyphicon-refresh:before {
  content: "\e031";
}
.glyphicon-list-alt:before {
  content: "\e032";
}
.glyphicon-lock:before {
  content: "\e033";
}
.glyphicon-flag:before {
  content: "\e034";
}
.glyphicon-headphones:before {
  content: "\e035";
}
.glyphicon-volume-off:before {
  content: "\e036";
}
.glyphicon-volume-down:before {
  content: "\e037";
}
.glyphicon-volume-up:before {
  content: "\e038";
}
.glyphicon-qrcode:before {
  content: "\e039";
}
.glyphicon-barcode:before {
  content: "\e040";
}
.glyphicon-tag:before {
  content: "\e041";
}
.glyphicon-tags:before {
  content: "\e042";
}
.glyphicon-book:before {
  content: "\e043";
}
.glyphicon-bookmark:before {
  content: "\e044";
}
.glyphicon-print:before {
  content: "\e045";
}
.glyphicon-camera:before {
  content: "\e046";
}
.glyphicon-font:before {
  content: "\e047";
}
.glyphicon-bold:before {
  content: "\e048";
}
.glyphicon-italic:before {
  content: "\e049";
}
.glyphicon-text-height:before {
  content: "\e050";
}
.glyphicon-text-width:before {
  content: "\e051";
}
.glyphicon-align-left:before {
  content: "\e052";
}
.glyphicon-align-center:before {
  content: "\e053";
}
.glyphicon-align-right:before {
  content: "\e054";
}
.glyphicon-align-justify:before {
  content: "\e055";
}
.glyphicon-list:before {
  content: "\e056";
}
.glyphicon-indent-left:before {
  content: "\e057";
}
.glyphicon-indent-right:before {
  content: "\e058";
}
.glyphicon-facetime-video:before {
  content: "\e059";
}
.glyphicon-picture:before {
  content: "\e060";
}
.glyphicon-map-marker:before {
  content: "\e062";
}
.glyphicon-adjust:before {
  content: "\e063";
}
.glyphicon-tint:before {
  content: "\e064";
}
.glyphicon-edit:before {
  content: "\e065";
}
.glyphicon-share:before {
  content: "\e066";
}
.glyphicon-check:before {
  content: "\e067";
}
.glyphicon-move:before {
  content: "\e068";
}
.glyphicon-step-backward:before {
  content: "\e069";
}
.glyphicon-fast-backward:before {
  content: "\e070";
}
.glyphicon-backward:before {
  content: "\e071";
}
.glyphicon-play:before {
  content: "\e072";
}
.glyphicon-pause:before {
  content: "\e073";
}
.glyphicon-stop:before {
  content: "\e074";
}
.glyphicon-forward:before {
  content: "\e075";
}
.glyphicon-fast-forward:before {
  content: "\e076";
}
.glyphicon-step-forward:before {
  content: "\e077";
}
.glyphicon-eject:before {
  content: "\e078";
}
.glyphicon-chevron-left:before {
  content: "\e079";
}
.glyphicon-chevron-right:before {
  content: "\e080";
}
.glyphicon-plus-sign:before {
  content: "\e081";
}
.glyphicon-minus-sign:before {
  content: "\e082";
}
.glyphicon-remove-sign:before {
  content: "\e083";
}
.glyphicon-ok-sign:before {
  content: "\e084";
}
.glyphicon-question-sign:before {
  content: "\e085";
}
.glyphicon-info-sign:before {
  content: "\e086";
}
.glyphicon-screenshot:before {
  content: "\e087";
}
.glyphicon-remove-circle:before {
  content: "\e088";
}
.glyphicon-ok-circle:before {
  content: "\e089";
}
.glyphicon-ban-circle:before {
  content: "\e090";
}
.glyphicon-arrow-left:before {
  content: "\e091";
}
.glyphicon-arrow-right:before {
  content: "\e092";
}
.glyphicon-arrow-up:before {
  content: "\e093";
}
.glyphicon-arrow-down:before {
  content: "\e094";
}
.glyphicon-share-alt:before {
  content: "\e095";
}
.glyphicon-resize-full:before {
  content: "\e096";
}
.glyphicon-resize-small:before {
  content: "\e097";
}
.glyphicon-exclamation-sign:before {
  content: "\e101";
}
.glyphicon-gift:before {
  content: "\e102";
}
.glyphicon-leaf:before {
  content: "\e103";
}
.glyphicon-fire:before {
  content: "\e104";
}
.glyphicon-eye-open:before {
  content: "\e105";
}
.glyphicon-eye-close:before {
  content: "\e106";
}
.glyphicon-warning-sign:before {
  content: "\e107";
}
.glyphicon-plane:before {
  content: "\e108";
}
.glyphicon-calendar:before {
  content: "\e109";
}
.glyphicon-random:before {
  content: "\e110";
}
.glyphicon-comment:before {
  content: "\e111";
}
.glyphicon-magnet:before {
  content: "\e112";
}
.glyphicon-chevron-up:before {
  content: "\e113";
}
.glyphicon-chevron-down:before {
  content: "\e114";
}
.glyphicon-retweet:before {
  content: "\e115";
}
.glyphicon-shopping-cart:before {
  content: "\e116";
}
.glyphicon-folder-close:before {
  content: "\e117";
}
.glyphicon-folder-open:before {
  content: "\e118";
}
.glyphicon-resize-vertical:before {
  content: "\e119";
}
.glyphicon-resize-horizontal:before {
  content: "\e120";
}
.glyphicon-hdd:before {
  content: "\e121";
}
.glyphicon-bullhorn:before {
  content: "\e122";
}
.glyphicon-bell:before {
  content: "\e123";
}
.glyphicon-certificate:before {
  content: "\e124";
}
.glyphicon-thumbs-up:before {
  content: "\e125";
}
.glyphicon-thumbs-down:before {
  content: "\e126";
}
.glyphicon-hand-right:before {
  content: "\e127";
}
.glyphicon-hand-left:before {
  content: "\e128";
}
.glyphicon-hand-up:before {
  content: "\e129";
}
.glyphicon-hand-down:before {
  content: "\e130";
}
.glyphicon-circle-arrow-right:before {
  content: "\e131";
}
.glyphicon-circle-arrow-left:before {
  content: "\e132";
}
.glyphicon-circle-arrow-up:before {
  content: "\e133";
}
.glyphicon-circle-arrow-down:before {
  content: "\e134";
}
.glyphicon-globe:before {
  content: "\e135";
}
.glyphicon-wrench:before {
  content: "\e136";
}
.glyphicon-tasks:before {
  content: "\e137";
}
.glyphicon-filter:before {
  content: "\e138";
}
.glyphicon-briefcase:before {
  content: "\e139";
}
.glyphicon-fullscreen:before {
  content: "\e140";
}
.glyphicon-dashboard:before {
  content: "\e141";
}
.glyphicon-paperclip:before {
  content: "\e142";
}
.glyphicon-heart-empty:before {
  content: "\e143";
}
.glyphicon-link:before {
  content: "\e144";
}
.glyphicon-phone:before {
  content: "\e145";
}
.glyphicon-pushpin:before {
  content: "\e146";
}
.glyphicon-usd:before {
  content: "\e148";
}
.glyphicon-gbp:before {
  content: "\e149";
}
.glyphicon-sort:before {
  content: "\e150";
}
.glyphicon-sort-by-alphabet:before {
  content: "\e151";
}
.glyphicon-sort-by-alphabet-alt:before {
  content: "\e152";
}
.glyphicon-sort-by-order:before {
  content: "\e153";
}
.glyphicon-sort-by-order-alt:before {
  content: "\e154";
}
.glyphicon-sort-by-attributes:before {
  content: "\e155";
}
.glyphicon-sort-by-attributes-alt:before {
  content: "\e156";
}
.glyphicon-unchecked:before {
  content: "\e157";
}
.glyphicon-expand:before {
  content: "\e158";
}
.glyphicon-collapse-down:before {
  content: "\e159";
}
.glyphicon-collapse-up:before {
  content: "\e160";
}
.glyphicon-log-in:before {
  content: "\e161";
}
.glyphicon-flash:before {
  content: "\e162";
}
.glyphicon-log-out:before {
  content: "\e163";
}
.glyphicon-new-window:before {
  content: "\e164";
}
.glyphicon-record:before {
  content: "\e165";
}
.glyphicon-save:before {
  content: "\e166";
}
.glyphicon-open:before {
  content: "\e167";
}
.glyphicon-saved:before {
  content: "\e168";
}
.glyphicon-import:before {
  content: "\e169";
}
.glyphicon-export:before {
  content: "\e170";
}
.glyphicon-send:before {
  content: "\e171";
}
.glyphicon-floppy-disk:before {
  content: "\e172";
}
.glyphicon-floppy-saved:before {
  content: "\e173";
}
.glyphicon-floppy-remove:before {
  content: "\e174";
}
.glyphicon-floppy-save:before {
  content: "\e175";
}
.glyphicon-floppy-open:before {
  content: "\e176";
}
.glyphicon-credit-card:before {
  content: "\e177";
}
.glyphicon-transfer:before {
  content: "\e178";
}
.glyphicon-cutlery:before {
  content: "\e179";
}
.glyphicon-header:before {
  content: "\e180";
}
.glyphicon-compressed:before {
  content: "\e181";
}
.glyphicon-earphone:before {
  content: "\e182";
}
.glyphicon-phone-alt:before {
  content: "\e183";
}
.glyphicon-tower:before {
  content: "\e184";
}
.glyphicon-stats:before {
  content: "\e185";
}
.glyphicon-sd-video:before {
  content: "\e186";
}
.glyphicon-hd-video:before {
  content: "\e187";
}
.glyphicon-subtitles:before {
  content: "\e188";
}
.glyphicon-sound-stereo:before {
  content: "\e189";
}
.glyphicon-sound-dolby:before {
  content: "\e190";
}
.glyphicon-sound-5-1:before {
  content: "\e191";
}
.glyphicon-sound-6-1:before {
  content: "\e192";
}
.glyphicon-sound-7-1:before {
  content: "\e193";
}
.glyphicon-copyright-mark:before {
  content: "\e194";
}
.glyphicon-registration-mark:before {
  content: "\e195";
}
.glyphicon-cloud-download:before {
  content: "\e197";
}
.glyphicon-cloud-upload:before {
  content: "\e198";
}
.glyphicon-tree-conifer:before {
  content: "\e199";
}
.glyphicon-tree-deciduous:before {
  content: "\e200";
}
.glyphicon-cd:before {
  content: "\e201";
}
.glyphicon-save-file:before {
  content: "\e202";
}
.glyphicon-open-file:before {
  content: "\e203";
}
.glyphicon-level-up:before {
  content: "\e204";
}
.glyphicon-copy:before {
  content: "\e205";
}
.glyphicon-paste:before {
  content: "\e206";
}
.glyphicon-alert:before {
  content: "\e209";
}
.glyphicon-equalizer:before {
  content: "\e210";
}
.glyphicon-king:before {
  content: "\e211";
}
.glyphicon-queen:before {
  content: "\e212";
}
.glyphicon-pawn:before {
  content: "\e213";
}
.glyphicon-bishop:before {
  content: "\e214";
}
.glyphicon-knight:before {
  content: "\e215";
}
.glyphicon-baby-formula:before {
  content: "\e216";
}
.glyphicon-tent:before {
  content: "\26fa";
}
.glyphicon-blackboard:before {
  content: "\e218";
}
.glyphicon-bed:before {
  content: "\e219";
}
.glyphicon-apple:before {
  content: "\f8ff";
}
.glyphicon-erase:before {
  content: "\e221";
}
.glyphicon-hourglass:before {
  content: "\231b";
}
.glyphicon-lamp:before {
  content: "\e223";
}
.glyphicon-duplicate:before {
  content: "\e224";
}
.glyphicon-piggy-bank:before {
  content: "\e225";
}
.glyphicon-scissors:before {
  content: "\e226";
}
.glyphicon-bitcoin:before {
  content: "\e227";
}
.glyphicon-btc:before {
  content: "\e227";
}
.glyphicon-xbt:before {
  content: "\e227";
}
.glyphicon-yen:before {
  content: "\00a5";
}
.glyphicon-jpy:before {
  content: "\00a5";
}
.glyphicon-ruble:before {
  content: "\20bd";
}
.glyphicon-rub:before {
  content: "\20bd";
}
.glyphicon-scale:before {
  content: "\e230";
}
.glyphicon-ice-lolly:before {
  content: "\e231";
}
.glyphicon-ice-lolly-tasted:before {
  content: "\e232";
}
.glyphicon-education:before {
  content: "\e233";
}
.glyphicon-option-horizontal:before {
  content: "\e234";
}
.glyphicon-option-vertical:before {
  content: "\e235";
}
.glyphicon-menu-hamburger:before {
  content: "\e236";
}
.glyphicon-modal-window:before {
  content: "\e237";
}
.glyphicon-oil:before {
  content: "\e238";
}
.glyphicon-grain:before {
  content: "\e239";
}
.glyphicon-sunglasses:before {
  content: "\e240";
}
.glyphicon-text-size:before {
  content: "\e241";
}
.glyphicon-text-color:before {
  content: "\e242";
}
.glyphicon-text-background:before {
  content: "\e243";
}
.glyphicon-object-align-top:before {
  content: "\e244";
}
.glyphicon-object-align-bottom:before {
  content: "\e245";
}
.glyphicon-object-align-horizontal:before {
  content: "\e246";
}
.glyphicon-object-align-left:before {
  content: "\e247";
}
.glyphicon-object-align-vertical:before {
  content: "\e248";
}
.glyphicon-object-align-right:before {
  content: "\e249";
}
.glyphicon-triangle-right:before {
  content: "\e250";
}
.glyphicon-triangle-left:before {
  content: "\e251";
}
.glyphicon-triangle-bottom:before {
  content: "\e252";
}
.glyphicon-triangle-top:before {
  content: "\e253";
}
.glyphicon-console:before {
  content: "\e254";
}
.glyphicon-superscript:before {
  content: "\e255";
}
.glyphicon-subscript:before {
  content: "\e256";
}
.glyphicon-menu-left:before {
  content: "\e257";
}
.glyphicon-menu-right:before {
  content: "\e258";
}
.glyphicon-menu-down:before {
  content: "\e259";
}
.glyphicon-menu-up:before {
  content: "\e260";
}
* {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
*:before,
*:after {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
html {
  font-size: 10px;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}
body {
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-size: 13px;
  line-height: 1.42857143;
  color: #000;
  background-color: #fff;
}
input,
button,
select,
textarea {
  font-family: inherit;
  font-size: inherit;
  line-height: inherit;
}
a {
  color: #337ab7;
  text-decoration: none;
}
a:hover,
a:focus {
  color: #23527c;
  text-decoration: underline;
}
a:focus {
  outline: 5px auto -webkit-focus-ring-color;
  outline-offset: -2px;
}
figure {
  margin: 0;
}
img {
  vertical-align: middle;
}
.img-responsive,
.thumbnail > img,
.thumbnail a > img,
.carousel-inner > .item > img,
.carousel-inner > .item > a > img {
  display: block;
  max-width: 100%;
  height: auto;
}
.img-rounded {
  border-radius: 3px;
}
.img-thumbnail {
  padding: 4px;
  line-height: 1.42857143;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 2px;
  -webkit-transition: all 0.2s ease-in-out;
  -o-transition: all 0.2s ease-in-out;
  transition: all 0.2s ease-in-out;
  display: inline-block;
  max-width: 100%;
  height: auto;
}
.img-circle {
  border-radius: 50%;
}
hr {
  margin-top: 18px;
  margin-bottom: 18px;
  border: 0;
  border-top: 1px solid #eeeeee;
}
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  padding: 0;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}
.sr-only-focusable:active,
.sr-only-focusable:focus {
  position: static;
  width: auto;
  height: auto;
  margin: 0;
  overflow: visible;
  clip: auto;
}
[role="button"] {
  cursor: pointer;
}
h1,
h2,
h3,
h4,
h5,
h6,
.h1,
.h2,
.h3,
.h4,
.h5,
.h6 {
  font-family: inherit;
  font-weight: 500;
  line-height: 1.1;
  color: inherit;
}
h1 small,
h2 small,
h3 small,
h4 small,
h5 small,
h6 small,
.h1 small,
.h2 small,
.h3 small,
.h4 small,
.h5 small,
.h6 small,
h1 .small,
h2 .small,
h3 .small,
h4 .small,
h5 .small,
h6 .small,
.h1 .small,
.h2 .small,
.h3 .small,
.h4 .small,
.h5 .small,
.h6 .small {
  font-weight: normal;
  line-height: 1;
  color: #777777;
}
h1,
.h1,
h2,
.h2,
h3,
.h3 {
  margin-top: 18px;
  margin-bottom: 9px;
}
h1 small,
.h1 small,
h2 small,
.h2 small,
h3 small,
.h3 small,
h1 .small,
.h1 .small,
h2 .small,
.h2 .small,
h3 .small,
.h3 .small {
  font-size: 65%;
}
h4,
.h4,
h5,
.h5,
h6,
.h6 {
  margin-top: 9px;
  margin-bottom: 9px;
}
h4 small,
.h4 small,
h5 small,
.h5 small,
h6 small,
.h6 small,
h4 .small,
.h4 .small,
h5 .small,
.h5 .small,
h6 .small,
.h6 .small {
  font-size: 75%;
}
h1,
.h1 {
  font-size: 33px;
}
h2,
.h2 {
  font-size: 27px;
}
h3,
.h3 {
  font-size: 23px;
}
h4,
.h4 {
  font-size: 17px;
}
h5,
.h5 {
  font-size: 13px;
}
h6,
.h6 {
  font-size: 12px;
}
p {
  margin: 0 0 9px;
}
.lead {
  margin-bottom: 18px;
  font-size: 14px;
  font-weight: 300;
  line-height: 1.4;
}
@media (min-width: 768px) {
  .lead {
    font-size: 19.5px;
  }
}
small,
.small {
  font-size: 92%;
}
mark,
.mark {
  background-color: #fcf8e3;
  padding: .2em;
}
.text-left {
  text-align: left;
}
.text-right {
  text-align: right;
}
.text-center {
  text-align: center;
}
.text-justify {
  text-align: justify;
}
.text-nowrap {
  white-space: nowrap;
}
.text-lowercase {
  text-transform: lowercase;
}
.text-uppercase {
  text-transform: uppercase;
}
.text-capitalize {
  text-transform: capitalize;
}
.text-muted {
  color: #777777;
}
.text-primary {
  color: #337ab7;
}
a.text-primary:hover,
a.text-primary:focus {
  color: #286090;
}
.text-success {
  color: #3c763d;
}
a.text-success:hover,
a.text-success:focus {
  color: #2b542c;
}
.text-info {
  color: #31708f;
}
a.text-info:hover,
a.text-info:focus {
  color: #245269;
}
.text-warning {
  color: #8a6d3b;
}
a.text-warning:hover,
a.text-warning:focus {
  color: #66512c;
}
.text-danger {
  color: #a94442;
}
a.text-danger:hover,
a.text-danger:focus {
  color: #843534;
}
.bg-primary {
  color: #fff;
  background-color: #337ab7;
}
a.bg-primary:hover,
a.bg-primary:focus {
  background-color: #286090;
}
.bg-success {
  background-color: #dff0d8;
}
a.bg-success:hover,
a.bg-success:focus {
  background-color: #c1e2b3;
}
.bg-info {
  background-color: #d9edf7;
}
a.bg-info:hover,
a.bg-info:focus {
  background-color: #afd9ee;
}
.bg-warning {
  background-color: #fcf8e3;
}
a.bg-warning:hover,
a.bg-warning:focus {
  background-color: #f7ecb5;
}
.bg-danger {
  background-color: #f2dede;
}
a.bg-danger:hover,
a.bg-danger:focus {
  background-color: #e4b9b9;
}
.page-header {
  padding-bottom: 8px;
  margin: 36px 0 18px;
  border-bottom: 1px solid #eeeeee;
}
ul,
ol {
  margin-top: 0;
  margin-bottom: 9px;
}
ul ul,
ol ul,
ul ol,
ol ol {
  margin-bottom: 0;
}
.list-unstyled {
  padding-left: 0;
  list-style: none;
}
.list-inline {
  padding-left: 0;
  list-style: none;
  margin-left: -5px;
}
.list-inline > li {
  display: inline-block;
  padding-left: 5px;
  padding-right: 5px;
}
dl {
  margin-top: 0;
  margin-bottom: 18px;
}
dt,
dd {
  line-height: 1.42857143;
}
dt {
  font-weight: bold;
}
dd {
  margin-left: 0;
}
@media (min-width: 541px) {
  .dl-horizontal dt {
    float: left;
    width: 160px;
    clear: left;
    text-align: right;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .dl-horizontal dd {
    margin-left: 180px;
  }
}
abbr[title],
abbr[data-original-title] {
  cursor: help;
  border-bottom: 1px dotted #777777;
}
.initialism {
  font-size: 90%;
  text-transform: uppercase;
}
blockquote {
  padding: 9px 18px;
  margin: 0 0 18px;
  font-size: inherit;
  border-left: 5px solid #eeeeee;
}
blockquote p:last-child,
blockquote ul:last-child,
blockquote ol:last-child {
  margin-bottom: 0;
}
blockquote footer,
blockquote small,
blockquote .small {
  display: block;
  font-size: 80%;
  line-height: 1.42857143;
  color: #777777;
}
blockquote footer:before,
blockquote small:before,
blockquote .small:before {
  content: '\2014 \00A0';
}
.blockquote-reverse,
blockquote.pull-right {
  padding-right: 15px;
  padding-left: 0;
  border-right: 5px solid #eeeeee;
  border-left: 0;
  text-align: right;
}
.blockquote-reverse footer:before,
blockquote.pull-right footer:before,
.blockquote-reverse small:before,
blockquote.pull-right small:before,
.blockquote-reverse .small:before,
blockquote.pull-right .small:before {
  content: '';
}
.blockquote-reverse footer:after,
blockquote.pull-right footer:after,
.blockquote-reverse small:after,
blockquote.pull-right small:after,
.blockquote-reverse .small:after,
blockquote.pull-right .small:after {
  content: '\00A0 \2014';
}
address {
  margin-bottom: 18px;
  font-style: normal;
  line-height: 1.42857143;
}
code,
kbd,
pre,
samp {
  font-family: monospace;
}
code {
  padding: 2px 4px;
  font-size: 90%;
  color: #c7254e;
  background-color: #f9f2f4;
  border-radius: 2px;
}
kbd {
  padding: 2px 4px;
  font-size: 90%;
  color: #888;
  background-color: transparent;
  border-radius: 1px;
  box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.25);
}
kbd kbd {
  padding: 0;
  font-size: 100%;
  font-weight: bold;
  box-shadow: none;
}
pre {
  display: block;
  padding: 8.5px;
  margin: 0 0 9px;
  font-size: 12px;
  line-height: 1.42857143;
  word-break: break-all;
  word-wrap: break-word;
  color: #333333;
  background-color: #f5f5f5;
  border: 1px solid #ccc;
  border-radius: 2px;
}
pre code {
  padding: 0;
  font-size: inherit;
  color: inherit;
  white-space: pre-wrap;
  background-color: transparent;
  border-radius: 0;
}
.pre-scrollable {
  max-height: 340px;
  overflow-y: scroll;
}
.container {
  margin-right: auto;
  margin-left: auto;
  padding-left: 0px;
  padding-right: 0px;
}
@media (min-width: 768px) {
  .container {
    width: 768px;
  }
}
@media (min-width: 992px) {
  .container {
    width: 940px;
  }
}
@media (min-width: 1200px) {
  .container {
    width: 1140px;
  }
}
.container-fluid {
  margin-right: auto;
  margin-left: auto;
  padding-left: 0px;
  padding-right: 0px;
}
.row {
  margin-left: 0px;
  margin-right: 0px;
}
.col-xs-1, .col-sm-1, .col-md-1, .col-lg-1, .col-xs-2, .col-sm-2, .col-md-2, .col-lg-2, .col-xs-3, .col-sm-3, .col-md-3, .col-lg-3, .col-xs-4, .col-sm-4, .col-md-4, .col-lg-4, .col-xs-5, .col-sm-5, .col-md-5, .col-lg-5, .col-xs-6, .col-sm-6, .col-md-6, .col-lg-6, .col-xs-7, .col-sm-7, .col-md-7, .col-lg-7, .col-xs-8, .col-sm-8, .col-md-8, .col-lg-8, .col-xs-9, .col-sm-9, .col-md-9, .col-lg-9, .col-xs-10, .col-sm-10, .col-md-10, .col-lg-10, .col-xs-11, .col-sm-11, .col-md-11, .col-lg-11, .col-xs-12, .col-sm-12, .col-md-12, .col-lg-12 {
  position: relative;
  min-height: 1px;
  padding-left: 0px;
  padding-right: 0px;
}
.col-xs-1, .col-xs-2, .col-xs-3, .col-xs-4, .col-xs-5, .col-xs-6, .col-xs-7, .col-xs-8, .col-xs-9, .col-xs-10, .col-xs-11, .col-xs-12 {
  float: left;
}
.col-xs-12 {
  width: 100%;
}
.col-xs-11 {
  width: 91.66666667%;
}
.col-xs-10 {
  width: 83.33333333%;
}
.col-xs-9 {
  width: 75%;
}
.col-xs-8 {
  width: 66.66666667%;
}
.col-xs-7 {
  width: 58.33333333%;
}
.col-xs-6 {
  width: 50%;
}
.col-xs-5 {
  width: 41.66666667%;
}
.col-xs-4 {
  width: 33.33333333%;
}
.col-xs-3 {
  width: 25%;
}
.col-xs-2 {
  width: 16.66666667%;
}
.col-xs-1 {
  width: 8.33333333%;
}
.col-xs-pull-12 {
  right: 100%;
}
.col-xs-pull-11 {
  right: 91.66666667%;
}
.col-xs-pull-10 {
  right: 83.33333333%;
}
.col-xs-pull-9 {
  right: 75%;
}
.col-xs-pull-8 {
  right: 66.66666667%;
}
.col-xs-pull-7 {
  right: 58.33333333%;
}
.col-xs-pull-6 {
  right: 50%;
}
.col-xs-pull-5 {
  right: 41.66666667%;
}
.col-xs-pull-4 {
  right: 33.33333333%;
}
.col-xs-pull-3 {
  right: 25%;
}
.col-xs-pull-2 {
  right: 16.66666667%;
}
.col-xs-pull-1 {
  right: 8.33333333%;
}
.col-xs-pull-0 {
  right: auto;
}
.col-xs-push-12 {
  left: 100%;
}
.col-xs-push-11 {
  left: 91.66666667%;
}
.col-xs-push-10 {
  left: 83.33333333%;
}
.col-xs-push-9 {
  left: 75%;
}
.col-xs-push-8 {
  left: 66.66666667%;
}
.col-xs-push-7 {
  left: 58.33333333%;
}
.col-xs-push-6 {
  left: 50%;
}
.col-xs-push-5 {
  left: 41.66666667%;
}
.col-xs-push-4 {
  left: 33.33333333%;
}
.col-xs-push-3 {
  left: 25%;
}
.col-xs-push-2 {
  left: 16.66666667%;
}
.col-xs-push-1 {
  left: 8.33333333%;
}
.col-xs-push-0 {
  left: auto;
}
.col-xs-offset-12 {
  margin-left: 100%;
}
.col-xs-offset-11 {
  margin-left: 91.66666667%;
}
.col-xs-offset-10 {
  margin-left: 83.33333333%;
}
.col-xs-offset-9 {
  margin-left: 75%;
}
.col-xs-offset-8 {
  margin-left: 66.66666667%;
}
.col-xs-offset-7 {
  margin-left: 58.33333333%;
}
.col-xs-offset-6 {
  margin-left: 50%;
}
.col-xs-offset-5 {
  margin-left: 41.66666667%;
}
.col-xs-offset-4 {
  margin-left: 33.33333333%;
}
.col-xs-offset-3 {
  margin-left: 25%;
}
.col-xs-offset-2 {
  margin-left: 16.66666667%;
}
.col-xs-offset-1 {
  margin-left: 8.33333333%;
}
.col-xs-offset-0 {
  margin-left: 0%;
}
@media (min-width: 768px) {
  .col-sm-1, .col-sm-2, .col-sm-3, .col-sm-4, .col-sm-5, .col-sm-6, .col-sm-7, .col-sm-8, .col-sm-9, .col-sm-10, .col-sm-11, .col-sm-12 {
    float: left;
  }
  .col-sm-12 {
    width: 100%;
  }
  .col-sm-11 {
    width: 91.66666667%;
  }
  .col-sm-10 {
    width: 83.33333333%;
  }
  .col-sm-9 {
    width: 75%;
  }
  .col-sm-8 {
    width: 66.66666667%;
  }
  .col-sm-7 {
    width: 58.33333333%;
  }
  .col-sm-6 {
    width: 50%;
  }
  .col-sm-5 {
    width: 41.66666667%;
  }
  .col-sm-4 {
    width: 33.33333333%;
  }
  .col-sm-3 {
    width: 25%;
  }
  .col-sm-2 {
    width: 16.66666667%;
  }
  .col-sm-1 {
    width: 8.33333333%;
  }
  .col-sm-pull-12 {
    right: 100%;
  }
  .col-sm-pull-11 {
    right: 91.66666667%;
  }
  .col-sm-pull-10 {
    right: 83.33333333%;
  }
  .col-sm-pull-9 {
    right: 75%;
  }
  .col-sm-pull-8 {
    right: 66.66666667%;
  }
  .col-sm-pull-7 {
    right: 58.33333333%;
  }
  .col-sm-pull-6 {
    right: 50%;
  }
  .col-sm-pull-5 {
    right: 41.66666667%;
  }
  .col-sm-pull-4 {
    right: 33.33333333%;
  }
  .col-sm-pull-3 {
    right: 25%;
  }
  .col-sm-pull-2 {
    right: 16.66666667%;
  }
  .col-sm-pull-1 {
    right: 8.33333333%;
  }
  .col-sm-pull-0 {
    right: auto;
  }
  .col-sm-push-12 {
    left: 100%;
  }
  .col-sm-push-11 {
    left: 91.66666667%;
  }
  .col-sm-push-10 {
    left: 83.33333333%;
  }
  .col-sm-push-9 {
    left: 75%;
  }
  .col-sm-push-8 {
    left: 66.66666667%;
  }
  .col-sm-push-7 {
    left: 58.33333333%;
  }
  .col-sm-push-6 {
    left: 50%;
  }
  .col-sm-push-5 {
    left: 41.66666667%;
  }
  .col-sm-push-4 {
    left: 33.33333333%;
  }
  .col-sm-push-3 {
    left: 25%;
  }
  .col-sm-push-2 {
    left: 16.66666667%;
  }
  .col-sm-push-1 {
    left: 8.33333333%;
  }
  .col-sm-push-0 {
    left: auto;
  }
  .col-sm-offset-12 {
    margin-left: 100%;
  }
  .col-sm-offset-11 {
    margin-left: 91.66666667%;
  }
  .col-sm-offset-10 {
    margin-left: 83.33333333%;
  }
  .col-sm-offset-9 {
    margin-left: 75%;
  }
  .col-sm-offset-8 {
    margin-left: 66.66666667%;
  }
  .col-sm-offset-7 {
    margin-left: 58.33333333%;
  }
  .col-sm-offset-6 {
    margin-left: 50%;
  }
  .col-sm-offset-5 {
    margin-left: 41.66666667%;
  }
  .col-sm-offset-4 {
    margin-left: 33.33333333%;
  }
  .col-sm-offset-3 {
    margin-left: 25%;
  }
  .col-sm-offset-2 {
    margin-left: 16.66666667%;
  }
  .col-sm-offset-1 {
    margin-left: 8.33333333%;
  }
  .col-sm-offset-0 {
    margin-left: 0%;
  }
}
@media (min-width: 992px) {
  .col-md-1, .col-md-2, .col-md-3, .col-md-4, .col-md-5, .col-md-6, .col-md-7, .col-md-8, .col-md-9, .col-md-10, .col-md-11, .col-md-12 {
    float: left;
  }
  .col-md-12 {
    width: 100%;
  }
  .col-md-11 {
    width: 91.66666667%;
  }
  .col-md-10 {
    width: 83.33333333%;
  }
  .col-md-9 {
    width: 75%;
  }
  .col-md-8 {
    width: 66.66666667%;
  }
  .col-md-7 {
    width: 58.33333333%;
  }
  .col-md-6 {
    width: 50%;
  }
  .col-md-5 {
    width: 41.66666667%;
  }
  .col-md-4 {
    width: 33.33333333%;
  }
  .col-md-3 {
    width: 25%;
  }
  .col-md-2 {
    width: 16.66666667%;
  }
  .col-md-1 {
    width: 8.33333333%;
  }
  .col-md-pull-12 {
    right: 100%;
  }
  .col-md-pull-11 {
    right: 91.66666667%;
  }
  .col-md-pull-10 {
    right: 83.33333333%;
  }
  .col-md-pull-9 {
    right: 75%;
  }
  .col-md-pull-8 {
    right: 66.66666667%;
  }
  .col-md-pull-7 {
    right: 58.33333333%;
  }
  .col-md-pull-6 {
    right: 50%;
  }
  .col-md-pull-5 {
    right: 41.66666667%;
  }
  .col-md-pull-4 {
    right: 33.33333333%;
  }
  .col-md-pull-3 {
    right: 25%;
  }
  .col-md-pull-2 {
    right: 16.66666667%;
  }
  .col-md-pull-1 {
    right: 8.33333333%;
  }
  .col-md-pull-0 {
    right: auto;
  }
  .col-md-push-12 {
    left: 100%;
  }
  .col-md-push-11 {
    left: 91.66666667%;
  }
  .col-md-push-10 {
    left: 83.33333333%;
  }
  .col-md-push-9 {
    left: 75%;
  }
  .col-md-push-8 {
    left: 66.66666667%;
  }
  .col-md-push-7 {
    left: 58.33333333%;
  }
  .col-md-push-6 {
    left: 50%;
  }
  .col-md-push-5 {
    left: 41.66666667%;
  }
  .col-md-push-4 {
    left: 33.33333333%;
  }
  .col-md-push-3 {
    left: 25%;
  }
  .col-md-push-2 {
    left: 16.66666667%;
  }
  .col-md-push-1 {
    left: 8.33333333%;
  }
  .col-md-push-0 {
    left: auto;
  }
  .col-md-offset-12 {
    margin-left: 100%;
  }
  .col-md-offset-11 {
    margin-left: 91.66666667%;
  }
  .col-md-offset-10 {
    margin-left: 83.33333333%;
  }
  .col-md-offset-9 {
    margin-left: 75%;
  }
  .col-md-offset-8 {
    margin-left: 66.66666667%;
  }
  .col-md-offset-7 {
    margin-left: 58.33333333%;
  }
  .col-md-offset-6 {
    margin-left: 50%;
  }
  .col-md-offset-5 {
    margin-left: 41.66666667%;
  }
  .col-md-offset-4 {
    margin-left: 33.33333333%;
  }
  .col-md-offset-3 {
    margin-left: 25%;
  }
  .col-md-offset-2 {
    margin-left: 16.66666667%;
  }
  .col-md-offset-1 {
    margin-left: 8.33333333%;
  }
  .col-md-offset-0 {
    margin-left: 0%;
  }
}
@media (min-width: 1200px) {
  .col-lg-1, .col-lg-2, .col-lg-3, .col-lg-4, .col-lg-5, .col-lg-6, .col-lg-7, .col-lg-8, .col-lg-9, .col-lg-10, .col-lg-11, .col-lg-12 {
    float: left;
  }
  .col-lg-12 {
    width: 100%;
  }
  .col-lg-11 {
    width: 91.66666667%;
  }
  .col-lg-10 {
    width: 83.33333333%;
  }
  .col-lg-9 {
    width: 75%;
  }
  .col-lg-8 {
    width: 66.66666667%;
  }
  .col-lg-7 {
    width: 58.33333333%;
  }
  .col-lg-6 {
    width: 50%;
  }
  .col-lg-5 {
    width: 41.66666667%;
  }
  .col-lg-4 {
    width: 33.33333333%;
  }
  .col-lg-3 {
    width: 25%;
  }
  .col-lg-2 {
    width: 16.66666667%;
  }
  .col-lg-1 {
    width: 8.33333333%;
  }
  .col-lg-pull-12 {
    right: 100%;
  }
  .col-lg-pull-11 {
    right: 91.66666667%;
  }
  .col-lg-pull-10 {
    right: 83.33333333%;
  }
  .col-lg-pull-9 {
    right: 75%;
  }
  .col-lg-pull-8 {
    right: 66.66666667%;
  }
  .col-lg-pull-7 {
    right: 58.33333333%;
  }
  .col-lg-pull-6 {
    right: 50%;
  }
  .col-lg-pull-5 {
    right: 41.66666667%;
  }
  .col-lg-pull-4 {
    right: 33.33333333%;
  }
  .col-lg-pull-3 {
    right: 25%;
  }
  .col-lg-pull-2 {
    right: 16.66666667%;
  }
  .col-lg-pull-1 {
    right: 8.33333333%;
  }
  .col-lg-pull-0 {
    right: auto;
  }
  .col-lg-push-12 {
    left: 100%;
  }
  .col-lg-push-11 {
    left: 91.66666667%;
  }
  .col-lg-push-10 {
    left: 83.33333333%;
  }
  .col-lg-push-9 {
    left: 75%;
  }
  .col-lg-push-8 {
    left: 66.66666667%;
  }
  .col-lg-push-7 {
    left: 58.33333333%;
  }
  .col-lg-push-6 {
    left: 50%;
  }
  .col-lg-push-5 {
    left: 41.66666667%;
  }
  .col-lg-push-4 {
    left: 33.33333333%;
  }
  .col-lg-push-3 {
    left: 25%;
  }
  .col-lg-push-2 {
    left: 16.66666667%;
  }
  .col-lg-push-1 {
    left: 8.33333333%;
  }
  .col-lg-push-0 {
    left: auto;
  }
  .col-lg-offset-12 {
    margin-left: 100%;
  }
  .col-lg-offset-11 {
    margin-left: 91.66666667%;
  }
  .col-lg-offset-10 {
    margin-left: 83.33333333%;
  }
  .col-lg-offset-9 {
    margin-left: 75%;
  }
  .col-lg-offset-8 {
    margin-left: 66.66666667%;
  }
  .col-lg-offset-7 {
    margin-left: 58.33333333%;
  }
  .col-lg-offset-6 {
    margin-left: 50%;
  }
  .col-lg-offset-5 {
    margin-left: 41.66666667%;
  }
  .col-lg-offset-4 {
    margin-left: 33.33333333%;
  }
  .col-lg-offset-3 {
    margin-left: 25%;
  }
  .col-lg-offset-2 {
    margin-left: 16.66666667%;
  }
  .col-lg-offset-1 {
    margin-left: 8.33333333%;
  }
  .col-lg-offset-0 {
    margin-left: 0%;
  }
}
table {
  background-color: transparent;
}
caption {
  padding-top: 8px;
  padding-bottom: 8px;
  color: #777777;
  text-align: left;
}
th {
  text-align: left;
}
.table {
  width: 100%;
  max-width: 100%;
  margin-bottom: 18px;
}
.table > thead > tr > th,
.table > tbody > tr > th,
.table > tfoot > tr > th,
.table > thead > tr > td,
.table > tbody > tr > td,
.table > tfoot > tr > td {
  padding: 8px;
  line-height: 1.42857143;
  vertical-align: top;
  border-top: 1px solid #ddd;
}
.table > thead > tr > th {
  vertical-align: bottom;
  border-bottom: 2px solid #ddd;
}
.table > caption + thead > tr:first-child > th,
.table > colgroup + thead > tr:first-child > th,
.table > thead:first-child > tr:first-child > th,
.table > caption + thead > tr:first-child > td,
.table > colgroup + thead > tr:first-child > td,
.table > thead:first-child > tr:first-child > td {
  border-top: 0;
}
.table > tbody + tbody {
  border-top: 2px solid #ddd;
}
.table .table {
  background-color: #fff;
}
.table-condensed > thead > tr > th,
.table-condensed > tbody > tr > th,
.table-condensed > tfoot > tr > th,
.table-condensed > thead > tr > td,
.table-condensed > tbody > tr > td,
.table-condensed > tfoot > tr > td {
  padding: 5px;
}
.table-bordered {
  border: 1px solid #ddd;
}
.table-bordered > thead > tr > th,
.table-bordered > tbody > tr > th,
.table-bordered > tfoot > tr > th,
.table-bordered > thead > tr > td,
.table-bordered > tbody > tr > td,
.table-bordered > tfoot > tr > td {
  border: 1px solid #ddd;
}
.table-bordered > thead > tr > th,
.table-bordered > thead > tr > td {
  border-bottom-width: 2px;
}
.table-striped > tbody > tr:nth-of-type(odd) {
  background-color: #f9f9f9;
}
.table-hover > tbody > tr:hover {
  background-color: #f5f5f5;
}
table col[class*="col-"] {
  position: static;
  float: none;
  display: table-column;
}
table td[class*="col-"],
table th[class*="col-"] {
  position: static;
  float: none;
  display: table-cell;
}
.table > thead > tr > td.active,
.table > tbody > tr > td.active,
.table > tfoot > tr > td.active,
.table > thead > tr > th.active,
.table > tbody > tr > th.active,
.table > tfoot > tr > th.active,
.table > thead > tr.active > td,
.table > tbody > tr.active > td,
.table > tfoot > tr.active > td,
.table > thead > tr.active > th,
.table > tbody > tr.active > th,
.table > tfoot > tr.active > th {
  background-color: #f5f5f5;
}
.table-hover > tbody > tr > td.active:hover,
.table-hover > tbody > tr > th.active:hover,
.table-hover > tbody > tr.active:hover > td,
.table-hover > tbody > tr:hover > .active,
.table-hover > tbody > tr.active:hover > th {
  background-color: #e8e8e8;
}
.table > thead > tr > td.success,
.table > tbody > tr > td.success,
.table > tfoot > tr > td.success,
.table > thead > tr > th.success,
.table > tbody > tr > th.success,
.table > tfoot > tr > th.success,
.table > thead > tr.success > td,
.table > tbody > tr.success > td,
.table > tfoot > tr.success > td,
.table > thead > tr.success > th,
.table > tbody > tr.success > th,
.table > tfoot > tr.success > th {
  background-color: #dff0d8;
}
.table-hover > tbody > tr > td.success:hover,
.table-hover > tbody > tr > th.success:hover,
.table-hover > tbody > tr.success:hover > td,
.table-hover > tbody > tr:hover > .success,
.table-hover > tbody > tr.success:hover > th {
  background-color: #d0e9c6;
}
.table > thead > tr > td.info,
.table > tbody > tr > td.info,
.table > tfoot > tr > td.info,
.table > thead > tr > th.info,
.table > tbody > tr > th.info,
.table > tfoot > tr > th.info,
.table > thead > tr.info > td,
.table > tbody > tr.info > td,
.table > tfoot > tr.info > td,
.table > thead > tr.info > th,
.table > tbody > tr.info > th,
.table > tfoot > tr.info > th {
  background-color: #d9edf7;
}
.table-hover > tbody > tr > td.info:hover,
.table-hover > tbody > tr > th.info:hover,
.table-hover > tbody > tr.info:hover > td,
.table-hover > tbody > tr:hover > .info,
.table-hover > tbody > tr.info:hover > th {
  background-color: #c4e3f3;
}
.table > thead > tr > td.warning,
.table > tbody > tr > td.warning,
.table > tfoot > tr > td.warning,
.table > thead > tr > th.warning,
.table > tbody > tr > th.warning,
.table > tfoot > tr > th.warning,
.table > thead > tr.warning > td,
.table > tbody > tr.warning > td,
.table > tfoot > tr.warning > td,
.table > thead > tr.warning > th,
.table > tbody > tr.warning > th,
.table > tfoot > tr.warning > th {
  background-color: #fcf8e3;
}
.table-hover > tbody > tr > td.warning:hover,
.table-hover > tbody > tr > th.warning:hover,
.table-hover > tbody > tr.warning:hover > td,
.table-hover > tbody > tr:hover > .warning,
.table-hover > tbody > tr.warning:hover > th {
  background-color: #faf2cc;
}
.table > thead > tr > td.danger,
.table > tbody > tr > td.danger,
.table > tfoot > tr > td.danger,
.table > thead > tr > th.danger,
.table > tbody > tr > th.danger,
.table > tfoot > tr > th.danger,
.table > thead > tr.danger > td,
.table > tbody > tr.danger > td,
.table > tfoot > tr.danger > td,
.table > thead > tr.danger > th,
.table > tbody > tr.danger > th,
.table > tfoot > tr.danger > th {
  background-color: #f2dede;
}
.table-hover > tbody > tr > td.danger:hover,
.table-hover > tbody > tr > th.danger:hover,
.table-hover > tbody > tr.danger:hover > td,
.table-hover > tbody > tr:hover > .danger,
.table-hover > tbody > tr.danger:hover > th {
  background-color: #ebcccc;
}
.table-responsive {
  overflow-x: auto;
  min-height: 0.01%;
}
@media screen and (max-width: 767px) {
  .table-responsive {
    width: 100%;
    margin-bottom: 13.5px;
    overflow-y: hidden;
    -ms-overflow-style: -ms-autohiding-scrollbar;
    border: 1px solid #ddd;
  }
  .table-responsive > .table {
    margin-bottom: 0;
  }
  .table-responsive > .table > thead > tr > th,
  .table-responsive > .table > tbody > tr > th,
  .table-responsive > .table > tfoot > tr > th,
  .table-responsive > .table > thead > tr > td,
  .table-responsive > .table > tbody > tr > td,
  .table-responsive > .table > tfoot > tr > td {
    white-space: nowrap;
  }
  .table-responsive > .table-bordered {
    border: 0;
  }
  .table-responsive > .table-bordered > thead > tr > th:first-child,
  .table-responsive > .table-bordered > tbody > tr > th:first-child,
  .table-responsive > .table-bordered > tfoot > tr > th:first-child,
  .table-responsive > .table-bordered > thead > tr > td:first-child,
  .table-responsive > .table-bordered > tbody > tr > td:first-child,
  .table-responsive > .table-bordered > tfoot > tr > td:first-child {
    border-left: 0;
  }
  .table-responsive > .table-bordered > thead > tr > th:last-child,
  .table-responsive > .table-bordered > tbody > tr > th:last-child,
  .table-responsive > .table-bordered > tfoot > tr > th:last-child,
  .table-responsive > .table-bordered > thead > tr > td:last-child,
  .table-responsive > .table-bordered > tbody > tr > td:last-child,
  .table-responsive > .table-bordered > tfoot > tr > td:last-child {
    border-right: 0;
  }
  .table-responsive > .table-bordered > tbody > tr:last-child > th,
  .table-responsive > .table-bordered > tfoot > tr:last-child > th,
  .table-responsive > .table-bordered > tbody > tr:last-child > td,
  .table-responsive > .table-bordered > tfoot > tr:last-child > td {
    border-bottom: 0;
  }
}
fieldset {
  padding: 0;
  margin: 0;
  border: 0;
  min-width: 0;
}
legend {
  display: block;
  width: 100%;
  padding: 0;
  margin-bottom: 18px;
  font-size: 19.5px;
  line-height: inherit;
  color: #333333;
  border: 0;
  border-bottom: 1px solid #e5e5e5;
}
label {
  display: inline-block;
  max-width: 100%;
  margin-bottom: 5px;
  font-weight: bold;
}
input[type="search"] {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
input[type="radio"],
input[type="checkbox"] {
  margin: 4px 0 0;
  margin-top: 1px \9;
  line-height: normal;
}
input[type="file"] {
  display: block;
}
input[type="range"] {
  display: block;
  width: 100%;
}
select[multiple],
select[size] {
  height: auto;
}
input[type="file"]:focus,
input[type="radio"]:focus,
input[type="checkbox"]:focus {
  outline: 5px auto -webkit-focus-ring-color;
  outline-offset: -2px;
}
output {
  display: block;
  padding-top: 7px;
  font-size: 13px;
  line-height: 1.42857143;
  color: #555555;
}
.form-control {
  display: block;
  width: 100%;
  height: 32px;
  padding: 6px 12px;
  font-size: 13px;
  line-height: 1.42857143;
  color: #555555;
  background-color: #fff;
  background-image: none;
  border: 1px solid #ccc;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  -webkit-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  -o-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
}
.form-control:focus {
  border-color: #66afe9;
  outline: 0;
  -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
  box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
}
.form-control::-moz-placeholder {
  color: #999;
  opacity: 1;
}
.form-control:-ms-input-placeholder {
  color: #999;
}
.form-control::-webkit-input-placeholder {
  color: #999;
}
.form-control::-ms-expand {
  border: 0;
  background-color: transparent;
}
.form-control[disabled],
.form-control[readonly],
fieldset[disabled] .form-control {
  background-color: #eeeeee;
  opacity: 1;
}
.form-control[disabled],
fieldset[disabled] .form-control {
  cursor: not-allowed;
}
textarea.form-control {
  height: auto;
}
input[type="search"] {
  -webkit-appearance: none;
}
@media screen and (-webkit-min-device-pixel-ratio: 0) {
  input[type="date"].form-control,
  input[type="time"].form-control,
  input[type="datetime-local"].form-control,
  input[type="month"].form-control {
    line-height: 32px;
  }
  input[type="date"].input-sm,
  input[type="time"].input-sm,
  input[type="datetime-local"].input-sm,
  input[type="month"].input-sm,
  .input-group-sm input[type="date"],
  .input-group-sm input[type="time"],
  .input-group-sm input[type="datetime-local"],
  .input-group-sm input[type="month"] {
    line-height: 30px;
  }
  input[type="date"].input-lg,
  input[type="time"].input-lg,
  input[type="datetime-local"].input-lg,
  input[type="month"].input-lg,
  .input-group-lg input[type="date"],
  .input-group-lg input[type="time"],
  .input-group-lg input[type="datetime-local"],
  .input-group-lg input[type="month"] {
    line-height: 45px;
  }
}
.form-group {
  margin-bottom: 15px;
}
.radio,
.checkbox {
  position: relative;
  display: block;
  margin-top: 10px;
  margin-bottom: 10px;
}
.radio label,
.checkbox label {
  min-height: 18px;
  padding-left: 20px;
  margin-bottom: 0;
  font-weight: normal;
  cursor: pointer;
}
.radio input[type="radio"],
.radio-inline input[type="radio"],
.checkbox input[type="checkbox"],
.checkbox-inline input[type="checkbox"] {
  position: absolute;
  margin-left: -20px;
  margin-top: 4px \9;
}
.radio + .radio,
.checkbox + .checkbox {
  margin-top: -5px;
}
.radio-inline,
.checkbox-inline {
  position: relative;
  display: inline-block;
  padding-left: 20px;
  margin-bottom: 0;
  vertical-align: middle;
  font-weight: normal;
  cursor: pointer;
}
.radio-inline + .radio-inline,
.checkbox-inline + .checkbox-inline {
  margin-top: 0;
  margin-left: 10px;
}
input[type="radio"][disabled],
input[type="checkbox"][disabled],
input[type="radio"].disabled,
input[type="checkbox"].disabled,
fieldset[disabled] input[type="radio"],
fieldset[disabled] input[type="checkbox"] {
  cursor: not-allowed;
}
.radio-inline.disabled,
.checkbox-inline.disabled,
fieldset[disabled] .radio-inline,
fieldset[disabled] .checkbox-inline {
  cursor: not-allowed;
}
.radio.disabled label,
.checkbox.disabled label,
fieldset[disabled] .radio label,
fieldset[disabled] .checkbox label {
  cursor: not-allowed;
}
.form-control-static {
  padding-top: 7px;
  padding-bottom: 7px;
  margin-bottom: 0;
  min-height: 31px;
}
.form-control-static.input-lg,
.form-control-static.input-sm {
  padding-left: 0;
  padding-right: 0;
}
.input-sm {
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
select.input-sm {
  height: 30px;
  line-height: 30px;
}
textarea.input-sm,
select[multiple].input-sm {
  height: auto;
}
.form-group-sm .form-control {
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
.form-group-sm select.form-control {
  height: 30px;
  line-height: 30px;
}
.form-group-sm textarea.form-control,
.form-group-sm select[multiple].form-control {
  height: auto;
}
.form-group-sm .form-control-static {
  height: 30px;
  min-height: 30px;
  padding: 6px 10px;
  font-size: 12px;
  line-height: 1.5;
}
.input-lg {
  height: 45px;
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
select.input-lg {
  height: 45px;
  line-height: 45px;
}
textarea.input-lg,
select[multiple].input-lg {
  height: auto;
}
.form-group-lg .form-control {
  height: 45px;
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
.form-group-lg select.form-control {
  height: 45px;
  line-height: 45px;
}
.form-group-lg textarea.form-control,
.form-group-lg select[multiple].form-control {
  height: auto;
}
.form-group-lg .form-control-static {
  height: 45px;
  min-height: 35px;
  padding: 11px 16px;
  font-size: 17px;
  line-height: 1.3333333;
}
.has-feedback {
  position: relative;
}
.has-feedback .form-control {
  padding-right: 40px;
}
.form-control-feedback {
  position: absolute;
  top: 0;
  right: 0;
  z-index: 2;
  display: block;
  width: 32px;
  height: 32px;
  line-height: 32px;
  text-align: center;
  pointer-events: none;
}
.input-lg + .form-control-feedback,
.input-group-lg + .form-control-feedback,
.form-group-lg .form-control + .form-control-feedback {
  width: 45px;
  height: 45px;
  line-height: 45px;
}
.input-sm + .form-control-feedback,
.input-group-sm + .form-control-feedback,
.form-group-sm .form-control + .form-control-feedback {
  width: 30px;
  height: 30px;
  line-height: 30px;
}
.has-success .help-block,
.has-success .control-label,
.has-success .radio,
.has-success .checkbox,
.has-success .radio-inline,
.has-success .checkbox-inline,
.has-success.radio label,
.has-success.checkbox label,
.has-success.radio-inline label,
.has-success.checkbox-inline label {
  color: #3c763d;
}
.has-success .form-control {
  border-color: #3c763d;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
}
.has-success .form-control:focus {
  border-color: #2b542c;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #67b168;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #67b168;
}
.has-success .input-group-addon {
  color: #3c763d;
  border-color: #3c763d;
  background-color: #dff0d8;
}
.has-success .form-control-feedback {
  color: #3c763d;
}
.has-warning .help-block,
.has-warning .control-label,
.has-warning .radio,
.has-warning .checkbox,
.has-warning .radio-inline,
.has-warning .checkbox-inline,
.has-warning.radio label,
.has-warning.checkbox label,
.has-warning.radio-inline label,
.has-warning.checkbox-inline label {
  color: #8a6d3b;
}
.has-warning .form-control {
  border-color: #8a6d3b;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
}
.has-warning .form-control:focus {
  border-color: #66512c;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #c0a16b;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #c0a16b;
}
.has-warning .input-group-addon {
  color: #8a6d3b;
  border-color: #8a6d3b;
  background-color: #fcf8e3;
}
.has-warning .form-control-feedback {
  color: #8a6d3b;
}
.has-error .help-block,
.has-error .control-label,
.has-error .radio,
.has-error .checkbox,
.has-error .radio-inline,
.has-error .checkbox-inline,
.has-error.radio label,
.has-error.checkbox label,
.has-error.radio-inline label,
.has-error.checkbox-inline label {
  color: #a94442;
}
.has-error .form-control {
  border-color: #a94442;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
}
.has-error .form-control:focus {
  border-color: #843534;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #ce8483;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #ce8483;
}
.has-error .input-group-addon {
  color: #a94442;
  border-color: #a94442;
  background-color: #f2dede;
}
.has-error .form-control-feedback {
  color: #a94442;
}
.has-feedback label ~ .form-control-feedback {
  top: 23px;
}
.has-feedback label.sr-only ~ .form-control-feedback {
  top: 0;
}
.help-block {
  display: block;
  margin-top: 5px;
  margin-bottom: 10px;
  color: #404040;
}
@media (min-width: 768px) {
  .form-inline .form-group {
    display: inline-block;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .form-inline .form-control {
    display: inline-block;
    width: auto;
    vertical-align: middle;
  }
  .form-inline .form-control-static {
    display: inline-block;
  }
  .form-inline .input-group {
    display: inline-table;
    vertical-align: middle;
  }
  .form-inline .input-group .input-group-addon,
  .form-inline .input-group .input-group-btn,
  .form-inline .input-group .form-control {
    width: auto;
  }
  .form-inline .input-group > .form-control {
    width: 100%;
  }
  .form-inline .control-label {
    margin-bottom: 0;
    vertical-align: middle;
  }
  .form-inline .radio,
  .form-inline .checkbox {
    display: inline-block;
    margin-top: 0;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .form-inline .radio label,
  .form-inline .checkbox label {
    padding-left: 0;
  }
  .form-inline .radio input[type="radio"],
  .form-inline .checkbox input[type="checkbox"] {
    position: relative;
    margin-left: 0;
  }
  .form-inline .has-feedback .form-control-feedback {
    top: 0;
  }
}
.form-horizontal .radio,
.form-horizontal .checkbox,
.form-horizontal .radio-inline,
.form-horizontal .checkbox-inline {
  margin-top: 0;
  margin-bottom: 0;
  padding-top: 7px;
}
.form-horizontal .radio,
.form-horizontal .checkbox {
  min-height: 25px;
}
.form-horizontal .form-group {
  margin-left: 0px;
  margin-right: 0px;
}
@media (min-width: 768px) {
  .form-horizontal .control-label {
    text-align: right;
    margin-bottom: 0;
    padding-top: 7px;
  }
}
.form-horizontal .has-feedback .form-control-feedback {
  right: 0px;
}
@media (min-width: 768px) {
  .form-horizontal .form-group-lg .control-label {
    padding-top: 11px;
    font-size: 17px;
  }
}
@media (min-width: 768px) {
  .form-horizontal .form-group-sm .control-label {
    padding-top: 6px;
    font-size: 12px;
  }
}
.btn {
  display: inline-block;
  margin-bottom: 0;
  font-weight: normal;
  text-align: center;
  vertical-align: middle;
  touch-action: manipulation;
  cursor: pointer;
  background-image: none;
  border: 1px solid transparent;
  white-space: nowrap;
  padding: 6px 12px;
  font-size: 13px;
  line-height: 1.42857143;
  border-radius: 2px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.btn:focus,
.btn:active:focus,
.btn.active:focus,
.btn.focus,
.btn:active.focus,
.btn.active.focus {
  outline: 5px auto -webkit-focus-ring-color;
  outline-offset: -2px;
}
.btn:hover,
.btn:focus,
.btn.focus {
  color: #333;
  text-decoration: none;
}
.btn:active,
.btn.active {
  outline: 0;
  background-image: none;
  -webkit-box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
  box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
}
.btn.disabled,
.btn[disabled],
fieldset[disabled] .btn {
  cursor: not-allowed;
  opacity: 0.65;
  filter: alpha(opacity=65);
  -webkit-box-shadow: none;
  box-shadow: none;
}
a.btn.disabled,
fieldset[disabled] a.btn {
  pointer-events: none;
}
.btn-default {
  color: #333;
  background-color: #fff;
  border-color: #ccc;
}
.btn-default:focus,
.btn-default.focus {
  color: #333;
  background-color: #e6e6e6;
  border-color: #8c8c8c;
}
.btn-default:hover {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.btn-default:active,
.btn-default.active,
.open > .dropdown-toggle.btn-default {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.btn-default:active:hover,
.btn-default.active:hover,
.open > .dropdown-toggle.btn-default:hover,
.btn-default:active:focus,
.btn-default.active:focus,
.open > .dropdown-toggle.btn-default:focus,
.btn-default:active.focus,
.btn-default.active.focus,
.open > .dropdown-toggle.btn-default.focus {
  color: #333;
  background-color: #d4d4d4;
  border-color: #8c8c8c;
}
.btn-default:active,
.btn-default.active,
.open > .dropdown-toggle.btn-default {
  background-image: none;
}
.btn-default.disabled:hover,
.btn-default[disabled]:hover,
fieldset[disabled] .btn-default:hover,
.btn-default.disabled:focus,
.btn-default[disabled]:focus,
fieldset[disabled] .btn-default:focus,
.btn-default.disabled.focus,
.btn-default[disabled].focus,
fieldset[disabled] .btn-default.focus {
  background-color: #fff;
  border-color: #ccc;
}
.btn-default .badge {
  color: #fff;
  background-color: #333;
}
.btn-primary {
  color: #fff;
  background-color: #337ab7;
  border-color: #2e6da4;
}
.btn-primary:focus,
.btn-primary.focus {
  color: #fff;
  background-color: #286090;
  border-color: #122b40;
}
.btn-primary:hover {
  color: #fff;
  background-color: #286090;
  border-color: #204d74;
}
.btn-primary:active,
.btn-primary.active,
.open > .dropdown-toggle.btn-primary {
  color: #fff;
  background-color: #286090;
  border-color: #204d74;
}
.btn-primary:active:hover,
.btn-primary.active:hover,
.open > .dropdown-toggle.btn-primary:hover,
.btn-primary:active:focus,
.btn-primary.active:focus,
.open > .dropdown-toggle.btn-primary:focus,
.btn-primary:active.focus,
.btn-primary.active.focus,
.open > .dropdown-toggle.btn-primary.focus {
  color: #fff;
  background-color: #204d74;
  border-color: #122b40;
}
.btn-primary:active,
.btn-primary.active,
.open > .dropdown-toggle.btn-primary {
  background-image: none;
}
.btn-primary.disabled:hover,
.btn-primary[disabled]:hover,
fieldset[disabled] .btn-primary:hover,
.btn-primary.disabled:focus,
.btn-primary[disabled]:focus,
fieldset[disabled] .btn-primary:focus,
.btn-primary.disabled.focus,
.btn-primary[disabled].focus,
fieldset[disabled] .btn-primary.focus {
  background-color: #337ab7;
  border-color: #2e6da4;
}
.btn-primary .badge {
  color: #337ab7;
  background-color: #fff;
}
.btn-success {
  color: #fff;
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.btn-success:focus,
.btn-success.focus {
  color: #fff;
  background-color: #449d44;
  border-color: #255625;
}
.btn-success:hover {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.btn-success:active,
.btn-success.active,
.open > .dropdown-toggle.btn-success {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.btn-success:active:hover,
.btn-success.active:hover,
.open > .dropdown-toggle.btn-success:hover,
.btn-success:active:focus,
.btn-success.active:focus,
.open > .dropdown-toggle.btn-success:focus,
.btn-success:active.focus,
.btn-success.active.focus,
.open > .dropdown-toggle.btn-success.focus {
  color: #fff;
  background-color: #398439;
  border-color: #255625;
}
.btn-success:active,
.btn-success.active,
.open > .dropdown-toggle.btn-success {
  background-image: none;
}
.btn-success.disabled:hover,
.btn-success[disabled]:hover,
fieldset[disabled] .btn-success:hover,
.btn-success.disabled:focus,
.btn-success[disabled]:focus,
fieldset[disabled] .btn-success:focus,
.btn-success.disabled.focus,
.btn-success[disabled].focus,
fieldset[disabled] .btn-success.focus {
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.btn-success .badge {
  color: #5cb85c;
  background-color: #fff;
}
.btn-info {
  color: #fff;
  background-color: #5bc0de;
  border-color: #46b8da;
}
.btn-info:focus,
.btn-info.focus {
  color: #fff;
  background-color: #31b0d5;
  border-color: #1b6d85;
}
.btn-info:hover {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.btn-info:active,
.btn-info.active,
.open > .dropdown-toggle.btn-info {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.btn-info:active:hover,
.btn-info.active:hover,
.open > .dropdown-toggle.btn-info:hover,
.btn-info:active:focus,
.btn-info.active:focus,
.open > .dropdown-toggle.btn-info:focus,
.btn-info:active.focus,
.btn-info.active.focus,
.open > .dropdown-toggle.btn-info.focus {
  color: #fff;
  background-color: #269abc;
  border-color: #1b6d85;
}
.btn-info:active,
.btn-info.active,
.open > .dropdown-toggle.btn-info {
  background-image: none;
}
.btn-info.disabled:hover,
.btn-info[disabled]:hover,
fieldset[disabled] .btn-info:hover,
.btn-info.disabled:focus,
.btn-info[disabled]:focus,
fieldset[disabled] .btn-info:focus,
.btn-info.disabled.focus,
.btn-info[disabled].focus,
fieldset[disabled] .btn-info.focus {
  background-color: #5bc0de;
  border-color: #46b8da;
}
.btn-info .badge {
  color: #5bc0de;
  background-color: #fff;
}
.btn-warning {
  color: #fff;
  background-color: #f0ad4e;
  border-color: #eea236;
}
.btn-warning:focus,
.btn-warning.focus {
  color: #fff;
  background-color: #ec971f;
  border-color: #985f0d;
}
.btn-warning:hover {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.btn-warning:active,
.btn-warning.active,
.open > .dropdown-toggle.btn-warning {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.btn-warning:active:hover,
.btn-warning.active:hover,
.open > .dropdown-toggle.btn-warning:hover,
.btn-warning:active:focus,
.btn-warning.active:focus,
.open > .dropdown-toggle.btn-warning:focus,
.btn-warning:active.focus,
.btn-warning.active.focus,
.open > .dropdown-toggle.btn-warning.focus {
  color: #fff;
  background-color: #d58512;
  border-color: #985f0d;
}
.btn-warning:active,
.btn-warning.active,
.open > .dropdown-toggle.btn-warning {
  background-image: none;
}
.btn-warning.disabled:hover,
.btn-warning[disabled]:hover,
fieldset[disabled] .btn-warning:hover,
.btn-warning.disabled:focus,
.btn-warning[disabled]:focus,
fieldset[disabled] .btn-warning:focus,
.btn-warning.disabled.focus,
.btn-warning[disabled].focus,
fieldset[disabled] .btn-warning.focus {
  background-color: #f0ad4e;
  border-color: #eea236;
}
.btn-warning .badge {
  color: #f0ad4e;
  background-color: #fff;
}
.btn-danger {
  color: #fff;
  background-color: #d9534f;
  border-color: #d43f3a;
}
.btn-danger:focus,
.btn-danger.focus {
  color: #fff;
  background-color: #c9302c;
  border-color: #761c19;
}
.btn-danger:hover {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.btn-danger:active,
.btn-danger.active,
.open > .dropdown-toggle.btn-danger {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.btn-danger:active:hover,
.btn-danger.active:hover,
.open > .dropdown-toggle.btn-danger:hover,
.btn-danger:active:focus,
.btn-danger.active:focus,
.open > .dropdown-toggle.btn-danger:focus,
.btn-danger:active.focus,
.btn-danger.active.focus,
.open > .dropdown-toggle.btn-danger.focus {
  color: #fff;
  background-color: #ac2925;
  border-color: #761c19;
}
.btn-danger:active,
.btn-danger.active,
.open > .dropdown-toggle.btn-danger {
  background-image: none;
}
.btn-danger.disabled:hover,
.btn-danger[disabled]:hover,
fieldset[disabled] .btn-danger:hover,
.btn-danger.disabled:focus,
.btn-danger[disabled]:focus,
fieldset[disabled] .btn-danger:focus,
.btn-danger.disabled.focus,
.btn-danger[disabled].focus,
fieldset[disabled] .btn-danger.focus {
  background-color: #d9534f;
  border-color: #d43f3a;
}
.btn-danger .badge {
  color: #d9534f;
  background-color: #fff;
}
.btn-link {
  color: #337ab7;
  font-weight: normal;
  border-radius: 0;
}
.btn-link,
.btn-link:active,
.btn-link.active,
.btn-link[disabled],
fieldset[disabled] .btn-link {
  background-color: transparent;
  -webkit-box-shadow: none;
  box-shadow: none;
}
.btn-link,
.btn-link:hover,
.btn-link:focus,
.btn-link:active {
  border-color: transparent;
}
.btn-link:hover,
.btn-link:focus {
  color: #23527c;
  text-decoration: underline;
  background-color: transparent;
}
.btn-link[disabled]:hover,
fieldset[disabled] .btn-link:hover,
.btn-link[disabled]:focus,
fieldset[disabled] .btn-link:focus {
  color: #777777;
  text-decoration: none;
}
.btn-lg,
.btn-group-lg > .btn {
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
.btn-sm,
.btn-group-sm > .btn {
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
.btn-xs,
.btn-group-xs > .btn {
  padding: 1px 5px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
.btn-block {
  display: block;
  width: 100%;
}
.btn-block + .btn-block {
  margin-top: 5px;
}
input[type="submit"].btn-block,
input[type="reset"].btn-block,
input[type="button"].btn-block {
  width: 100%;
}
.fade {
  opacity: 0;
  -webkit-transition: opacity 0.15s linear;
  -o-transition: opacity 0.15s linear;
  transition: opacity 0.15s linear;
}
.fade.in {
  opacity: 1;
}
.collapse {
  display: none;
}
.collapse.in {
  display: block;
}
tr.collapse.in {
  display: table-row;
}
tbody.collapse.in {
  display: table-row-group;
}
.collapsing {
  position: relative;
  height: 0;
  overflow: hidden;
  -webkit-transition-property: height, visibility;
  transition-property: height, visibility;
  -webkit-transition-duration: 0.35s;
  transition-duration: 0.35s;
  -webkit-transition-timing-function: ease;
  transition-timing-function: ease;
}
.caret {
  display: inline-block;
  width: 0;
  height: 0;
  margin-left: 2px;
  vertical-align: middle;
  border-top: 4px dashed;
  border-top: 4px solid \9;
  border-right: 4px solid transparent;
  border-left: 4px solid transparent;
}
.dropup,
.dropdown {
  position: relative;
}
.dropdown-toggle:focus {
  outline: 0;
}
.dropdown-menu {
  position: absolute;
  top: 100%;
  left: 0;
  z-index: 1000;
  display: none;
  float: left;
  min-width: 160px;
  padding: 5px 0;
  margin: 2px 0 0;
  list-style: none;
  font-size: 13px;
  text-align: left;
  background-color: #fff;
  border: 1px solid #ccc;
  border: 1px solid rgba(0, 0, 0, 0.15);
  border-radius: 2px;
  -webkit-box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175);
  background-clip: padding-box;
}
.dropdown-menu.pull-right {
  right: 0;
  left: auto;
}
.dropdown-menu .divider {
  height: 1px;
  margin: 8px 0;
  overflow: hidden;
  background-color: #e5e5e5;
}
.dropdown-menu > li > a {
  display: block;
  padding: 3px 20px;
  clear: both;
  font-weight: normal;
  line-height: 1.42857143;
  color: #333333;
  white-space: nowrap;
}
.dropdown-menu > li > a:hover,
.dropdown-menu > li > a:focus {
  text-decoration: none;
  color: #262626;
  background-color: #f5f5f5;
}
.dropdown-menu > .active > a,
.dropdown-menu > .active > a:hover,
.dropdown-menu > .active > a:focus {
  color: #fff;
  text-decoration: none;
  outline: 0;
  background-color: #337ab7;
}
.dropdown-menu > .disabled > a,
.dropdown-menu > .disabled > a:hover,
.dropdown-menu > .disabled > a:focus {
  color: #777777;
}
.dropdown-menu > .disabled > a:hover,
.dropdown-menu > .disabled > a:focus {
  text-decoration: none;
  background-color: transparent;
  background-image: none;
  filter: progid:DXImageTransform.Microsoft.gradient(enabled = false);
  cursor: not-allowed;
}
.open > .dropdown-menu {
  display: block;
}
.open > a {
  outline: 0;
}
.dropdown-menu-right {
  left: auto;
  right: 0;
}
.dropdown-menu-left {
  left: 0;
  right: auto;
}
.dropdown-header {
  display: block;
  padding: 3px 20px;
  font-size: 12px;
  line-height: 1.42857143;
  color: #777777;
  white-space: nowrap;
}
.dropdown-backdrop {
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
  z-index: 990;
}
.pull-right > .dropdown-menu {
  right: 0;
  left: auto;
}
.dropup .caret,
.navbar-fixed-bottom .dropdown .caret {
  border-top: 0;
  border-bottom: 4px dashed;
  border-bottom: 4px solid \9;
  content: "";
}
.dropup .dropdown-menu,
.navbar-fixed-bottom .dropdown .dropdown-menu {
  top: auto;
  bottom: 100%;
  margin-bottom: 2px;
}
@media (min-width: 541px) {
  .navbar-right .dropdown-menu {
    left: auto;
    right: 0;
  }
  .navbar-right .dropdown-menu-left {
    left: 0;
    right: auto;
  }
}
.btn-group,
.btn-group-vertical {
  position: relative;
  display: inline-block;
  vertical-align: middle;
}
.btn-group > .btn,
.btn-group-vertical > .btn {
  position: relative;
  float: left;
}
.btn-group > .btn:hover,
.btn-group-vertical > .btn:hover,
.btn-group > .btn:focus,
.btn-group-vertical > .btn:focus,
.btn-group > .btn:active,
.btn-group-vertical > .btn:active,
.btn-group > .btn.active,
.btn-group-vertical > .btn.active {
  z-index: 2;
}
.btn-group .btn + .btn,
.btn-group .btn + .btn-group,
.btn-group .btn-group + .btn,
.btn-group .btn-group + .btn-group {
  margin-left: -1px;
}
.btn-toolbar {
  margin-left: -5px;
}
.btn-toolbar .btn,
.btn-toolbar .btn-group,
.btn-toolbar .input-group {
  float: left;
}
.btn-toolbar > .btn,
.btn-toolbar > .btn-group,
.btn-toolbar > .input-group {
  margin-left: 5px;
}
.btn-group > .btn:not(:first-child):not(:last-child):not(.dropdown-toggle) {
  border-radius: 0;
}
.btn-group > .btn:first-child {
  margin-left: 0;
}
.btn-group > .btn:first-child:not(:last-child):not(.dropdown-toggle) {
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}
.btn-group > .btn:last-child:not(:first-child),
.btn-group > .dropdown-toggle:not(:first-child) {
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}
.btn-group > .btn-group {
  float: left;
}
.btn-group > .btn-group:not(:first-child):not(:last-child) > .btn {
  border-radius: 0;
}
.btn-group > .btn-group:first-child:not(:last-child) > .btn:last-child,
.btn-group > .btn-group:first-child:not(:last-child) > .dropdown-toggle {
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}
.btn-group > .btn-group:last-child:not(:first-child) > .btn:first-child {
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}
.btn-group .dropdown-toggle:active,
.btn-group.open .dropdown-toggle {
  outline: 0;
}
.btn-group > .btn + .dropdown-toggle {
  padding-left: 8px;
  padding-right: 8px;
}
.btn-group > .btn-lg + .dropdown-toggle {
  padding-left: 12px;
  padding-right: 12px;
}
.btn-group.open .dropdown-toggle {
  -webkit-box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
  box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
}
.btn-group.open .dropdown-toggle.btn-link {
  -webkit-box-shadow: none;
  box-shadow: none;
}
.btn .caret {
  margin-left: 0;
}
.btn-lg .caret {
  border-width: 5px 5px 0;
  border-bottom-width: 0;
}
.dropup .btn-lg .caret {
  border-width: 0 5px 5px;
}
.btn-group-vertical > .btn,
.btn-group-vertical > .btn-group,
.btn-group-vertical > .btn-group > .btn {
  display: block;
  float: none;
  width: 100%;
  max-width: 100%;
}
.btn-group-vertical > .btn-group > .btn {
  float: none;
}
.btn-group-vertical > .btn + .btn,
.btn-group-vertical > .btn + .btn-group,
.btn-group-vertical > .btn-group + .btn,
.btn-group-vertical > .btn-group + .btn-group {
  margin-top: -1px;
  margin-left: 0;
}
.btn-group-vertical > .btn:not(:first-child):not(:last-child) {
  border-radius: 0;
}
.btn-group-vertical > .btn:first-child:not(:last-child) {
  border-top-right-radius: 2px;
  border-top-left-radius: 2px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.btn-group-vertical > .btn:last-child:not(:first-child) {
  border-top-right-radius: 0;
  border-top-left-radius: 0;
  border-bottom-right-radius: 2px;
  border-bottom-left-radius: 2px;
}
.btn-group-vertical > .btn-group:not(:first-child):not(:last-child) > .btn {
  border-radius: 0;
}
.btn-group-vertical > .btn-group:first-child:not(:last-child) > .btn:last-child,
.btn-group-vertical > .btn-group:first-child:not(:last-child) > .dropdown-toggle {
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.btn-group-vertical > .btn-group:last-child:not(:first-child) > .btn:first-child {
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.btn-group-justified {
  display: table;
  width: 100%;
  table-layout: fixed;
  border-collapse: separate;
}
.btn-group-justified > .btn,
.btn-group-justified > .btn-group {
  float: none;
  display: table-cell;
  width: 1%;
}
.btn-group-justified > .btn-group .btn {
  width: 100%;
}
.btn-group-justified > .btn-group .dropdown-menu {
  left: auto;
}
[data-toggle="buttons"] > .btn input[type="radio"],
[data-toggle="buttons"] > .btn-group > .btn input[type="radio"],
[data-toggle="buttons"] > .btn input[type="checkbox"],
[data-toggle="buttons"] > .btn-group > .btn input[type="checkbox"] {
  position: absolute;
  clip: rect(0, 0, 0, 0);
  pointer-events: none;
}
.input-group {
  position: relative;
  display: table;
  border-collapse: separate;
}
.input-group[class*="col-"] {
  float: none;
  padding-left: 0;
  padding-right: 0;
}
.input-group .form-control {
  position: relative;
  z-index: 2;
  float: left;
  width: 100%;
  margin-bottom: 0;
}
.input-group .form-control:focus {
  z-index: 3;
}
.input-group-lg > .form-control,
.input-group-lg > .input-group-addon,
.input-group-lg > .input-group-btn > .btn {
  height: 45px;
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
select.input-group-lg > .form-control,
select.input-group-lg > .input-group-addon,
select.input-group-lg > .input-group-btn > .btn {
  height: 45px;
  line-height: 45px;
}
textarea.input-group-lg > .form-control,
textarea.input-group-lg > .input-group-addon,
textarea.input-group-lg > .input-group-btn > .btn,
select[multiple].input-group-lg > .form-control,
select[multiple].input-group-lg > .input-group-addon,
select[multiple].input-group-lg > .input-group-btn > .btn {
  height: auto;
}
.input-group-sm > .form-control,
.input-group-sm > .input-group-addon,
.input-group-sm > .input-group-btn > .btn {
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
select.input-group-sm > .form-control,
select.input-group-sm > .input-group-addon,
select.input-group-sm > .input-group-btn > .btn {
  height: 30px;
  line-height: 30px;
}
textarea.input-group-sm > .form-control,
textarea.input-group-sm > .input-group-addon,
textarea.input-group-sm > .input-group-btn > .btn,
select[multiple].input-group-sm > .form-control,
select[multiple].input-group-sm > .input-group-addon,
select[multiple].input-group-sm > .input-group-btn > .btn {
  height: auto;
}
.input-group-addon,
.input-group-btn,
.input-group .form-control {
  display: table-cell;
}
.input-group-addon:not(:first-child):not(:last-child),
.input-group-btn:not(:first-child):not(:last-child),
.input-group .form-control:not(:first-child):not(:last-child) {
  border-radius: 0;
}
.input-group-addon,
.input-group-btn {
  width: 1%;
  white-space: nowrap;
  vertical-align: middle;
}
.input-group-addon {
  padding: 6px 12px;
  font-size: 13px;
  font-weight: normal;
  line-height: 1;
  color: #555555;
  text-align: center;
  background-color: #eeeeee;
  border: 1px solid #ccc;
  border-radius: 2px;
}
.input-group-addon.input-sm {
  padding: 5px 10px;
  font-size: 12px;
  border-radius: 1px;
}
.input-group-addon.input-lg {
  padding: 10px 16px;
  font-size: 17px;
  border-radius: 3px;
}
.input-group-addon input[type="radio"],
.input-group-addon input[type="checkbox"] {
  margin-top: 0;
}
.input-group .form-control:first-child,
.input-group-addon:first-child,
.input-group-btn:first-child > .btn,
.input-group-btn:first-child > .btn-group > .btn,
.input-group-btn:first-child > .dropdown-toggle,
.input-group-btn:last-child > .btn:not(:last-child):not(.dropdown-toggle),
.input-group-btn:last-child > .btn-group:not(:last-child) > .btn {
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}
.input-group-addon:first-child {
  border-right: 0;
}
.input-group .form-control:last-child,
.input-group-addon:last-child,
.input-group-btn:last-child > .btn,
.input-group-btn:last-child > .btn-group > .btn,
.input-group-btn:last-child > .dropdown-toggle,
.input-group-btn:first-child > .btn:not(:first-child),
.input-group-btn:first-child > .btn-group:not(:first-child) > .btn {
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}
.input-group-addon:last-child {
  border-left: 0;
}
.input-group-btn {
  position: relative;
  font-size: 0;
  white-space: nowrap;
}
.input-group-btn > .btn {
  position: relative;
}
.input-group-btn > .btn + .btn {
  margin-left: -1px;
}
.input-group-btn > .btn:hover,
.input-group-btn > .btn:focus,
.input-group-btn > .btn:active {
  z-index: 2;
}
.input-group-btn:first-child > .btn,
.input-group-btn:first-child > .btn-group {
  margin-right: -1px;
}
.input-group-btn:last-child > .btn,
.input-group-btn:last-child > .btn-group {
  z-index: 2;
  margin-left: -1px;
}
.nav {
  margin-bottom: 0;
  padding-left: 0;
  list-style: none;
}
.nav > li {
  position: relative;
  display: block;
}
.nav > li > a {
  position: relative;
  display: block;
  padding: 10px 15px;
}
.nav > li > a:hover,
.nav > li > a:focus {
  text-decoration: none;
  background-color: #eeeeee;
}
.nav > li.disabled > a {
  color: #777777;
}
.nav > li.disabled > a:hover,
.nav > li.disabled > a:focus {
  color: #777777;
  text-decoration: none;
  background-color: transparent;
  cursor: not-allowed;
}
.nav .open > a,
.nav .open > a:hover,
.nav .open > a:focus {
  background-color: #eeeeee;
  border-color: #337ab7;
}
.nav .nav-divider {
  height: 1px;
  margin: 8px 0;
  overflow: hidden;
  background-color: #e5e5e5;
}
.nav > li > a > img {
  max-width: none;
}
.nav-tabs {
  border-bottom: 1px solid #ddd;
}
.nav-tabs > li {
  float: left;
  margin-bottom: -1px;
}
.nav-tabs > li > a {
  margin-right: 2px;
  line-height: 1.42857143;
  border: 1px solid transparent;
  border-radius: 2px 2px 0 0;
}
.nav-tabs > li > a:hover {
  border-color: #eeeeee #eeeeee #ddd;
}
.nav-tabs > li.active > a,
.nav-tabs > li.active > a:hover,
.nav-tabs > li.active > a:focus {
  color: #555555;
  background-color: #fff;
  border: 1px solid #ddd;
  border-bottom-color: transparent;
  cursor: default;
}
.nav-tabs.nav-justified {
  width: 100%;
  border-bottom: 0;
}
.nav-tabs.nav-justified > li {
  float: none;
}
.nav-tabs.nav-justified > li > a {
  text-align: center;
  margin-bottom: 5px;
}
.nav-tabs.nav-justified > .dropdown .dropdown-menu {
  top: auto;
  left: auto;
}
@media (min-width: 768px) {
  .nav-tabs.nav-justified > li {
    display: table-cell;
    width: 1%;
  }
  .nav-tabs.nav-justified > li > a {
    margin-bottom: 0;
  }
}
.nav-tabs.nav-justified > li > a {
  margin-right: 0;
  border-radius: 2px;
}
.nav-tabs.nav-justified > .active > a,
.nav-tabs.nav-justified > .active > a:hover,
.nav-tabs.nav-justified > .active > a:focus {
  border: 1px solid #ddd;
}
@media (min-width: 768px) {
  .nav-tabs.nav-justified > li > a {
    border-bottom: 1px solid #ddd;
    border-radius: 2px 2px 0 0;
  }
  .nav-tabs.nav-justified > .active > a,
  .nav-tabs.nav-justified > .active > a:hover,
  .nav-tabs.nav-justified > .active > a:focus {
    border-bottom-color: #fff;
  }
}
.nav-pills > li {
  float: left;
}
.nav-pills > li > a {
  border-radius: 2px;
}
.nav-pills > li + li {
  margin-left: 2px;
}
.nav-pills > li.active > a,
.nav-pills > li.active > a:hover,
.nav-pills > li.active > a:focus {
  color: #fff;
  background-color: #337ab7;
}
.nav-stacked > li {
  float: none;
}
.nav-stacked > li + li {
  margin-top: 2px;
  margin-left: 0;
}
.nav-justified {
  width: 100%;
}
.nav-justified > li {
  float: none;
}
.nav-justified > li > a {
  text-align: center;
  margin-bottom: 5px;
}
.nav-justified > .dropdown .dropdown-menu {
  top: auto;
  left: auto;
}
@media (min-width: 768px) {
  .nav-justified > li {
    display: table-cell;
    width: 1%;
  }
  .nav-justified > li > a {
    margin-bottom: 0;
  }
}
.nav-tabs-justified {
  border-bottom: 0;
}
.nav-tabs-justified > li > a {
  margin-right: 0;
  border-radius: 2px;
}
.nav-tabs-justified > .active > a,
.nav-tabs-justified > .active > a:hover,
.nav-tabs-justified > .active > a:focus {
  border: 1px solid #ddd;
}
@media (min-width: 768px) {
  .nav-tabs-justified > li > a {
    border-bottom: 1px solid #ddd;
    border-radius: 2px 2px 0 0;
  }
  .nav-tabs-justified > .active > a,
  .nav-tabs-justified > .active > a:hover,
  .nav-tabs-justified > .active > a:focus {
    border-bottom-color: #fff;
  }
}
.tab-content > .tab-pane {
  display: none;
}
.tab-content > .active {
  display: block;
}
.nav-tabs .dropdown-menu {
  margin-top: -1px;
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.navbar {
  position: relative;
  min-height: 30px;
  margin-bottom: 18px;
  border: 1px solid transparent;
}
@media (min-width: 541px) {
  .navbar {
    border-radius: 2px;
  }
}
@media (min-width: 541px) {
  .navbar-header {
    float: left;
  }
}
.navbar-collapse {
  overflow-x: visible;
  padding-right: 0px;
  padding-left: 0px;
  border-top: 1px solid transparent;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.1);
  -webkit-overflow-scrolling: touch;
}
.navbar-collapse.in {
  overflow-y: auto;
}
@media (min-width: 541px) {
  .navbar-collapse {
    width: auto;
    border-top: 0;
    box-shadow: none;
  }
  .navbar-collapse.collapse {
    display: block !important;
    height: auto !important;
    padding-bottom: 0;
    overflow: visible !important;
  }
  .navbar-collapse.in {
    overflow-y: visible;
  }
  .navbar-fixed-top .navbar-collapse,
  .navbar-static-top .navbar-collapse,
  .navbar-fixed-bottom .navbar-collapse {
    padding-left: 0;
    padding-right: 0;
  }
}
.navbar-fixed-top .navbar-collapse,
.navbar-fixed-bottom .navbar-collapse {
  max-height: 340px;
}
@media (max-device-width: 540px) and (orientation: landscape) {
  .navbar-fixed-top .navbar-collapse,
  .navbar-fixed-bottom .navbar-collapse {
    max-height: 200px;
  }
}
.container > .navbar-header,
.container-fluid > .navbar-header,
.container > .navbar-collapse,
.container-fluid > .navbar-collapse {
  margin-right: 0px;
  margin-left: 0px;
}
@media (min-width: 541px) {
  .container > .navbar-header,
  .container-fluid > .navbar-header,
  .container > .navbar-collapse,
  .container-fluid > .navbar-collapse {
    margin-right: 0;
    margin-left: 0;
  }
}
.navbar-static-top {
  z-index: 1000;
  border-width: 0 0 1px;
}
@media (min-width: 541px) {
  .navbar-static-top {
    border-radius: 0;
  }
}
.navbar-fixed-top,
.navbar-fixed-bottom {
  position: fixed;
  right: 0;
  left: 0;
  z-index: 1030;
}
@media (min-width: 541px) {
  .navbar-fixed-top,
  .navbar-fixed-bottom {
    border-radius: 0;
  }
}
.navbar-fixed-top {
  top: 0;
  border-width: 0 0 1px;
}
.navbar-fixed-bottom {
  bottom: 0;
  margin-bottom: 0;
  border-width: 1px 0 0;
}
.navbar-brand {
  float: left;
  padding: 6px 0px;
  font-size: 17px;
  line-height: 18px;
  height: 30px;
}
.navbar-brand:hover,
.navbar-brand:focus {
  text-decoration: none;
}
.navbar-brand > img {
  display: block;
}
@media (min-width: 541px) {
  .navbar > .container .navbar-brand,
  .navbar > .container-fluid .navbar-brand {
    margin-left: 0px;
  }
}
.navbar-toggle {
  position: relative;
  float: right;
  margin-right: 0px;
  padding: 9px 10px;
  margin-top: -2px;
  margin-bottom: -2px;
  background-color: transparent;
  background-image: none;
  border: 1px solid transparent;
  border-radius: 2px;
}
.navbar-toggle:focus {
  outline: 0;
}
.navbar-toggle .icon-bar {
  display: block;
  width: 22px;
  height: 2px;
  border-radius: 1px;
}
.navbar-toggle .icon-bar + .icon-bar {
  margin-top: 4px;
}
@media (min-width: 541px) {
  .navbar-toggle {
    display: none;
  }
}
.navbar-nav {
  margin: 3px 0px;
}
.navbar-nav > li > a {
  padding-top: 10px;
  padding-bottom: 10px;
  line-height: 18px;
}
@media (max-width: 540px) {
  .navbar-nav .open .dropdown-menu {
    position: static;
    float: none;
    width: auto;
    margin-top: 0;
    background-color: transparent;
    border: 0;
    box-shadow: none;
  }
  .navbar-nav .open .dropdown-menu > li > a,
  .navbar-nav .open .dropdown-menu .dropdown-header {
    padding: 5px 15px 5px 25px;
  }
  .navbar-nav .open .dropdown-menu > li > a {
    line-height: 18px;
  }
  .navbar-nav .open .dropdown-menu > li > a:hover,
  .navbar-nav .open .dropdown-menu > li > a:focus {
    background-image: none;
  }
}
@media (min-width: 541px) {
  .navbar-nav {
    float: left;
    margin: 0;
  }
  .navbar-nav > li {
    float: left;
  }
  .navbar-nav > li > a {
    padding-top: 6px;
    padding-bottom: 6px;
  }
}
.navbar-form {
  margin-left: 0px;
  margin-right: 0px;
  padding: 10px 0px;
  border-top: 1px solid transparent;
  border-bottom: 1px solid transparent;
  -webkit-box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.1), 0 1px 0 rgba(255, 255, 255, 0.1);
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.1), 0 1px 0 rgba(255, 255, 255, 0.1);
  margin-top: -1px;
  margin-bottom: -1px;
}
@media (min-width: 768px) {
  .navbar-form .form-group {
    display: inline-block;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .navbar-form .form-control {
    display: inline-block;
    width: auto;
    vertical-align: middle;
  }
  .navbar-form .form-control-static {
    display: inline-block;
  }
  .navbar-form .input-group {
    display: inline-table;
    vertical-align: middle;
  }
  .navbar-form .input-group .input-group-addon,
  .navbar-form .input-group .input-group-btn,
  .navbar-form .input-group .form-control {
    width: auto;
  }
  .navbar-form .input-group > .form-control {
    width: 100%;
  }
  .navbar-form .control-label {
    margin-bottom: 0;
    vertical-align: middle;
  }
  .navbar-form .radio,
  .navbar-form .checkbox {
    display: inline-block;
    margin-top: 0;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .navbar-form .radio label,
  .navbar-form .checkbox label {
    padding-left: 0;
  }
  .navbar-form .radio input[type="radio"],
  .navbar-form .checkbox input[type="checkbox"] {
    position: relative;
    margin-left: 0;
  }
  .navbar-form .has-feedback .form-control-feedback {
    top: 0;
  }
}
@media (max-width: 540px) {
  .navbar-form .form-group {
    margin-bottom: 5px;
  }
  .navbar-form .form-group:last-child {
    margin-bottom: 0;
  }
}
@media (min-width: 541px) {
  .navbar-form {
    width: auto;
    border: 0;
    margin-left: 0;
    margin-right: 0;
    padding-top: 0;
    padding-bottom: 0;
    -webkit-box-shadow: none;
    box-shadow: none;
  }
}
.navbar-nav > li > .dropdown-menu {
  margin-top: 0;
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.navbar-fixed-bottom .navbar-nav > li > .dropdown-menu {
  margin-bottom: 0;
  border-top-right-radius: 2px;
  border-top-left-radius: 2px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.navbar-btn {
  margin-top: -1px;
  margin-bottom: -1px;
}
.navbar-btn.btn-sm {
  margin-top: 0px;
  margin-bottom: 0px;
}
.navbar-btn.btn-xs {
  margin-top: 4px;
  margin-bottom: 4px;
}
.navbar-text {
  margin-top: 6px;
  margin-bottom: 6px;
}
@media (min-width: 541px) {
  .navbar-text {
    float: left;
    margin-left: 0px;
    margin-right: 0px;
  }
}
@media (min-width: 541px) {
  .navbar-left {
    float: left !important;
    float: left;
  }
  .navbar-right {
    float: right !important;
    float: right;
    margin-right: 0px;
  }
  .navbar-right ~ .navbar-right {
    margin-right: 0;
  }
}
.navbar-default {
  background-color: #f8f8f8;
  border-color: #e7e7e7;
}
.navbar-default .navbar-brand {
  color: #777;
}
.navbar-default .navbar-brand:hover,
.navbar-default .navbar-brand:focus {
  color: #5e5e5e;
  background-color: transparent;
}
.navbar-default .navbar-text {
  color: #777;
}
.navbar-default .navbar-nav > li > a {
  color: #777;
}
.navbar-default .navbar-nav > li > a:hover,
.navbar-default .navbar-nav > li > a:focus {
  color: #333;
  background-color: transparent;
}
.navbar-default .navbar-nav > .active > a,
.navbar-default .navbar-nav > .active > a:hover,
.navbar-default .navbar-nav > .active > a:focus {
  color: #555;
  background-color: #e7e7e7;
}
.navbar-default .navbar-nav > .disabled > a,
.navbar-default .navbar-nav > .disabled > a:hover,
.navbar-default .navbar-nav > .disabled > a:focus {
  color: #ccc;
  background-color: transparent;
}
.navbar-default .navbar-toggle {
  border-color: #ddd;
}
.navbar-default .navbar-toggle:hover,
.navbar-default .navbar-toggle:focus {
  background-color: #ddd;
}
.navbar-default .navbar-toggle .icon-bar {
  background-color: #888;
}
.navbar-default .navbar-collapse,
.navbar-default .navbar-form {
  border-color: #e7e7e7;
}
.navbar-default .navbar-nav > .open > a,
.navbar-default .navbar-nav > .open > a:hover,
.navbar-default .navbar-nav > .open > a:focus {
  background-color: #e7e7e7;
  color: #555;
}
@media (max-width: 540px) {
  .navbar-default .navbar-nav .open .dropdown-menu > li > a {
    color: #777;
  }
  .navbar-default .navbar-nav .open .dropdown-menu > li > a:hover,
  .navbar-default .navbar-nav .open .dropdown-menu > li > a:focus {
    color: #333;
    background-color: transparent;
  }
  .navbar-default .navbar-nav .open .dropdown-menu > .active > a,
  .navbar-default .navbar-nav .open .dropdown-menu > .active > a:hover,
  .navbar-default .navbar-nav .open .dropdown-menu > .active > a:focus {
    color: #555;
    background-color: #e7e7e7;
  }
  .navbar-default .navbar-nav .open .dropdown-menu > .disabled > a,
  .navbar-default .navbar-nav .open .dropdown-menu > .disabled > a:hover,
  .navbar-default .navbar-nav .open .dropdown-menu > .disabled > a:focus {
    color: #ccc;
    background-color: transparent;
  }
}
.navbar-default .navbar-link {
  color: #777;
}
.navbar-default .navbar-link:hover {
  color: #333;
}
.navbar-default .btn-link {
  color: #777;
}
.navbar-default .btn-link:hover,
.navbar-default .btn-link:focus {
  color: #333;
}
.navbar-default .btn-link[disabled]:hover,
fieldset[disabled] .navbar-default .btn-link:hover,
.navbar-default .btn-link[disabled]:focus,
fieldset[disabled] .navbar-default .btn-link:focus {
  color: #ccc;
}
.navbar-inverse {
  background-color: #222;
  border-color: #080808;
}
.navbar-inverse .navbar-brand {
  color: #9d9d9d;
}
.navbar-inverse .navbar-brand:hover,
.navbar-inverse .navbar-brand:focus {
  color: #fff;
  background-color: transparent;
}
.navbar-inverse .navbar-text {
  color: #9d9d9d;
}
.navbar-inverse .navbar-nav > li > a {
  color: #9d9d9d;
}
.navbar-inverse .navbar-nav > li > a:hover,
.navbar-inverse .navbar-nav > li > a:focus {
  color: #fff;
  background-color: transparent;
}
.navbar-inverse .navbar-nav > .active > a,
.navbar-inverse .navbar-nav > .active > a:hover,
.navbar-inverse .navbar-nav > .active > a:focus {
  color: #fff;
  background-color: #080808;
}
.navbar-inverse .navbar-nav > .disabled > a,
.navbar-inverse .navbar-nav > .disabled > a:hover,
.navbar-inverse .navbar-nav > .disabled > a:focus {
  color: #444;
  background-color: transparent;
}
.navbar-inverse .navbar-toggle {
  border-color: #333;
}
.navbar-inverse .navbar-toggle:hover,
.navbar-inverse .navbar-toggle:focus {
  background-color: #333;
}
.navbar-inverse .navbar-toggle .icon-bar {
  background-color: #fff;
}
.navbar-inverse .navbar-collapse,
.navbar-inverse .navbar-form {
  border-color: #101010;
}
.navbar-inverse .navbar-nav > .open > a,
.navbar-inverse .navbar-nav > .open > a:hover,
.navbar-inverse .navbar-nav > .open > a:focus {
  background-color: #080808;
  color: #fff;
}
@media (max-width: 540px) {
  .navbar-inverse .navbar-nav .open .dropdown-menu > .dropdown-header {
    border-color: #080808;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu .divider {
    background-color: #080808;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > li > a {
    color: #9d9d9d;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > li > a:hover,
  .navbar-inverse .navbar-nav .open .dropdown-menu > li > a:focus {
    color: #fff;
    background-color: transparent;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > .active > a,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .active > a:hover,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .active > a:focus {
    color: #fff;
    background-color: #080808;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > .disabled > a,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .disabled > a:hover,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .disabled > a:focus {
    color: #444;
    background-color: transparent;
  }
}
.navbar-inverse .navbar-link {
  color: #9d9d9d;
}
.navbar-inverse .navbar-link:hover {
  color: #fff;
}
.navbar-inverse .btn-link {
  color: #9d9d9d;
}
.navbar-inverse .btn-link:hover,
.navbar-inverse .btn-link:focus {
  color: #fff;
}
.navbar-inverse .btn-link[disabled]:hover,
fieldset[disabled] .navbar-inverse .btn-link:hover,
.navbar-inverse .btn-link[disabled]:focus,
fieldset[disabled] .navbar-inverse .btn-link:focus {
  color: #444;
}
.breadcrumb {
  padding: 8px 15px;
  margin-bottom: 18px;
  list-style: none;
  background-color: #f5f5f5;
  border-radius: 2px;
}
.breadcrumb > li {
  display: inline-block;
}
.breadcrumb > li + li:before {
  content: "/\00a0";
  padding: 0 5px;
  color: #5e5e5e;
}
.breadcrumb > .active {
  color: #777777;
}
.pagination {
  display: inline-block;
  padding-left: 0;
  margin: 18px 0;
  border-radius: 2px;
}
.pagination > li {
  display: inline;
}
.pagination > li > a,
.pagination > li > span {
  position: relative;
  float: left;
  padding: 6px 12px;
  line-height: 1.42857143;
  text-decoration: none;
  color: #337ab7;
  background-color: #fff;
  border: 1px solid #ddd;
  margin-left: -1px;
}
.pagination > li:first-child > a,
.pagination > li:first-child > span {
  margin-left: 0;
  border-bottom-left-radius: 2px;
  border-top-left-radius: 2px;
}
.pagination > li:last-child > a,
.pagination > li:last-child > span {
  border-bottom-right-radius: 2px;
  border-top-right-radius: 2px;
}
.pagination > li > a:hover,
.pagination > li > span:hover,
.pagination > li > a:focus,
.pagination > li > span:focus {
  z-index: 2;
  color: #23527c;
  background-color: #eeeeee;
  border-color: #ddd;
}
.pagination > .active > a,
.pagination > .active > span,
.pagination > .active > a:hover,
.pagination > .active > span:hover,
.pagination > .active > a:focus,
.pagination > .active > span:focus {
  z-index: 3;
  color: #fff;
  background-color: #337ab7;
  border-color: #337ab7;
  cursor: default;
}
.pagination > .disabled > span,
.pagination > .disabled > span:hover,
.pagination > .disabled > span:focus,
.pagination > .disabled > a,
.pagination > .disabled > a:hover,
.pagination > .disabled > a:focus {
  color: #777777;
  background-color: #fff;
  border-color: #ddd;
  cursor: not-allowed;
}
.pagination-lg > li > a,
.pagination-lg > li > span {
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
}
.pagination-lg > li:first-child > a,
.pagination-lg > li:first-child > span {
  border-bottom-left-radius: 3px;
  border-top-left-radius: 3px;
}
.pagination-lg > li:last-child > a,
.pagination-lg > li:last-child > span {
  border-bottom-right-radius: 3px;
  border-top-right-radius: 3px;
}
.pagination-sm > li > a,
.pagination-sm > li > span {
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
}
.pagination-sm > li:first-child > a,
.pagination-sm > li:first-child > span {
  border-bottom-left-radius: 1px;
  border-top-left-radius: 1px;
}
.pagination-sm > li:last-child > a,
.pagination-sm > li:last-child > span {
  border-bottom-right-radius: 1px;
  border-top-right-radius: 1px;
}
.pager {
  padding-left: 0;
  margin: 18px 0;
  list-style: none;
  text-align: center;
}
.pager li {
  display: inline;
}
.pager li > a,
.pager li > span {
  display: inline-block;
  padding: 5px 14px;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 15px;
}
.pager li > a:hover,
.pager li > a:focus {
  text-decoration: none;
  background-color: #eeeeee;
}
.pager .next > a,
.pager .next > span {
  float: right;
}
.pager .previous > a,
.pager .previous > span {
  float: left;
}
.pager .disabled > a,
.pager .disabled > a:hover,
.pager .disabled > a:focus,
.pager .disabled > span {
  color: #777777;
  background-color: #fff;
  cursor: not-allowed;
}
.label {
  display: inline;
  padding: .2em .6em .3em;
  font-size: 75%;
  font-weight: bold;
  line-height: 1;
  color: #fff;
  text-align: center;
  white-space: nowrap;
  vertical-align: baseline;
  border-radius: .25em;
}
a.label:hover,
a.label:focus {
  color: #fff;
  text-decoration: none;
  cursor: pointer;
}
.label:empty {
  display: none;
}
.btn .label {
  position: relative;
  top: -1px;
}
.label-default {
  background-color: #777777;
}
.label-default[href]:hover,
.label-default[href]:focus {
  background-color: #5e5e5e;
}
.label-primary {
  background-color: #337ab7;
}
.label-primary[href]:hover,
.label-primary[href]:focus {
  background-color: #286090;
}
.label-success {
  background-color: #5cb85c;
}
.label-success[href]:hover,
.label-success[href]:focus {
  background-color: #449d44;
}
.label-info {
  background-color: #5bc0de;
}
.label-info[href]:hover,
.label-info[href]:focus {
  background-color: #31b0d5;
}
.label-warning {
  background-color: #f0ad4e;
}
.label-warning[href]:hover,
.label-warning[href]:focus {
  background-color: #ec971f;
}
.label-danger {
  background-color: #d9534f;
}
.label-danger[href]:hover,
.label-danger[href]:focus {
  background-color: #c9302c;
}
.badge {
  display: inline-block;
  min-width: 10px;
  padding: 3px 7px;
  font-size: 12px;
  font-weight: bold;
  color: #fff;
  line-height: 1;
  vertical-align: middle;
  white-space: nowrap;
  text-align: center;
  background-color: #777777;
  border-radius: 10px;
}
.badge:empty {
  display: none;
}
.btn .badge {
  position: relative;
  top: -1px;
}
.btn-xs .badge,
.btn-group-xs > .btn .badge {
  top: 0;
  padding: 1px 5px;
}
a.badge:hover,
a.badge:focus {
  color: #fff;
  text-decoration: none;
  cursor: pointer;
}
.list-group-item.active > .badge,
.nav-pills > .active > a > .badge {
  color: #337ab7;
  background-color: #fff;
}
.list-group-item > .badge {
  float: right;
}
.list-group-item > .badge + .badge {
  margin-right: 5px;
}
.nav-pills > li > a > .badge {
  margin-left: 3px;
}
.jumbotron {
  padding-top: 30px;
  padding-bottom: 30px;
  margin-bottom: 30px;
  color: inherit;
  background-color: #eeeeee;
}
.jumbotron h1,
.jumbotron .h1 {
  color: inherit;
}
.jumbotron p {
  margin-bottom: 15px;
  font-size: 20px;
  font-weight: 200;
}
.jumbotron > hr {
  border-top-color: #d5d5d5;
}
.container .jumbotron,
.container-fluid .jumbotron {
  border-radius: 3px;
  padding-left: 0px;
  padding-right: 0px;
}
.jumbotron .container {
  max-width: 100%;
}
@media screen and (min-width: 768px) {
  .jumbotron {
    padding-top: 48px;
    padding-bottom: 48px;
  }
  .container .jumbotron,
  .container-fluid .jumbotron {
    padding-left: 60px;
    padding-right: 60px;
  }
  .jumbotron h1,
  .jumbotron .h1 {
    font-size: 59px;
  }
}
.thumbnail {
  display: block;
  padding: 4px;
  margin-bottom: 18px;
  line-height: 1.42857143;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 2px;
  -webkit-transition: border 0.2s ease-in-out;
  -o-transition: border 0.2s ease-in-out;
  transition: border 0.2s ease-in-out;
}
.thumbnail > img,
.thumbnail a > img {
  margin-left: auto;
  margin-right: auto;
}
a.thumbnail:hover,
a.thumbnail:focus,
a.thumbnail.active {
  border-color: #337ab7;
}
.thumbnail .caption {
  padding: 9px;
  color: #000;
}
.alert {
  padding: 15px;
  margin-bottom: 18px;
  border: 1px solid transparent;
  border-radius: 2px;
}
.alert h4 {
  margin-top: 0;
  color: inherit;
}
.alert .alert-link {
  font-weight: bold;
}
.alert > p,
.alert > ul {
  margin-bottom: 0;
}
.alert > p + p {
  margin-top: 5px;
}
.alert-dismissable,
.alert-dismissible {
  padding-right: 35px;
}
.alert-dismissable .close,
.alert-dismissible .close {
  position: relative;
  top: -2px;
  right: -21px;
  color: inherit;
}
.alert-success {
  background-color: #dff0d8;
  border-color: #d6e9c6;
  color: #3c763d;
}
.alert-success hr {
  border-top-color: #c9e2b3;
}
.alert-success .alert-link {
  color: #2b542c;
}
.alert-info {
  background-color: #d9edf7;
  border-color: #bce8f1;
  color: #31708f;
}
.alert-info hr {
  border-top-color: #a6e1ec;
}
.alert-info .alert-link {
  color: #245269;
}
.alert-warning {
  background-color: #fcf8e3;
  border-color: #faebcc;
  color: #8a6d3b;
}
.alert-warning hr {
  border-top-color: #f7e1b5;
}
.alert-warning .alert-link {
  color: #66512c;
}
.alert-danger {
  background-color: #f2dede;
  border-color: #ebccd1;
  color: #a94442;
}
.alert-danger hr {
  border-top-color: #e4b9c0;
}
.alert-danger .alert-link {
  color: #843534;
}
@-webkit-keyframes progress-bar-stripes {
  from {
    background-position: 40px 0;
  }
  to {
    background-position: 0 0;
  }
}
@keyframes progress-bar-stripes {
  from {
    background-position: 40px 0;
  }
  to {
    background-position: 0 0;
  }
}
.progress {
  overflow: hidden;
  height: 18px;
  margin-bottom: 18px;
  background-color: #f5f5f5;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.1);
  box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.1);
}
.progress-bar {
  float: left;
  width: 0%;
  height: 100%;
  font-size: 12px;
  line-height: 18px;
  color: #fff;
  text-align: center;
  background-color: #337ab7;
  -webkit-box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.15);
  box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.15);
  -webkit-transition: width 0.6s ease;
  -o-transition: width 0.6s ease;
  transition: width 0.6s ease;
}
.progress-striped .progress-bar,
.progress-bar-striped {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-size: 40px 40px;
}
.progress.active .progress-bar,
.progress-bar.active {
  -webkit-animation: progress-bar-stripes 2s linear infinite;
  -o-animation: progress-bar-stripes 2s linear infinite;
  animation: progress-bar-stripes 2s linear infinite;
}
.progress-bar-success {
  background-color: #5cb85c;
}
.progress-striped .progress-bar-success {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.progress-bar-info {
  background-color: #5bc0de;
}
.progress-striped .progress-bar-info {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.progress-bar-warning {
  background-color: #f0ad4e;
}
.progress-striped .progress-bar-warning {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.progress-bar-danger {
  background-color: #d9534f;
}
.progress-striped .progress-bar-danger {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.media {
  margin-top: 15px;
}
.media:first-child {
  margin-top: 0;
}
.media,
.media-body {
  zoom: 1;
  overflow: hidden;
}
.media-body {
  width: 10000px;
}
.media-object {
  display: block;
}
.media-object.img-thumbnail {
  max-width: none;
}
.media-right,
.media > .pull-right {
  padding-left: 10px;
}
.media-left,
.media > .pull-left {
  padding-right: 10px;
}
.media-left,
.media-right,
.media-body {
  display: table-cell;
  vertical-align: top;
}
.media-middle {
  vertical-align: middle;
}
.media-bottom {
  vertical-align: bottom;
}
.media-heading {
  margin-top: 0;
  margin-bottom: 5px;
}
.media-list {
  padding-left: 0;
  list-style: none;
}
.list-group {
  margin-bottom: 20px;
  padding-left: 0;
}
.list-group-item {
  position: relative;
  display: block;
  padding: 10px 15px;
  margin-bottom: -1px;
  background-color: #fff;
  border: 1px solid #ddd;
}
.list-group-item:first-child {
  border-top-right-radius: 2px;
  border-top-left-radius: 2px;
}
.list-group-item:last-child {
  margin-bottom: 0;
  border-bottom-right-radius: 2px;
  border-bottom-left-radius: 2px;
}
a.list-group-item,
button.list-group-item {
  color: #555;
}
a.list-group-item .list-group-item-heading,
button.list-group-item .list-group-item-heading {
  color: #333;
}
a.list-group-item:hover,
button.list-group-item:hover,
a.list-group-item:focus,
button.list-group-item:focus {
  text-decoration: none;
  color: #555;
  background-color: #f5f5f5;
}
button.list-group-item {
  width: 100%;
  text-align: left;
}
.list-group-item.disabled,
.list-group-item.disabled:hover,
.list-group-item.disabled:focus {
  background-color: #eeeeee;
  color: #777777;
  cursor: not-allowed;
}
.list-group-item.disabled .list-group-item-heading,
.list-group-item.disabled:hover .list-group-item-heading,
.list-group-item.disabled:focus .list-group-item-heading {
  color: inherit;
}
.list-group-item.disabled .list-group-item-text,
.list-group-item.disabled:hover .list-group-item-text,
.list-group-item.disabled:focus .list-group-item-text {
  color: #777777;
}
.list-group-item.active,
.list-group-item.active:hover,
.list-group-item.active:focus {
  z-index: 2;
  color: #fff;
  background-color: #337ab7;
  border-color: #337ab7;
}
.list-group-item.active .list-group-item-heading,
.list-group-item.active:hover .list-group-item-heading,
.list-group-item.active:focus .list-group-item-heading,
.list-group-item.active .list-group-item-heading > small,
.list-group-item.active:hover .list-group-item-heading > small,
.list-group-item.active:focus .list-group-item-heading > small,
.list-group-item.active .list-group-item-heading > .small,
.list-group-item.active:hover .list-group-item-heading > .small,
.list-group-item.active:focus .list-group-item-heading > .small {
  color: inherit;
}
.list-group-item.active .list-group-item-text,
.list-group-item.active:hover .list-group-item-text,
.list-group-item.active:focus .list-group-item-text {
  color: #c7ddef;
}
.list-group-item-success {
  color: #3c763d;
  background-color: #dff0d8;
}
a.list-group-item-success,
button.list-group-item-success {
  color: #3c763d;
}
a.list-group-item-success .list-group-item-heading,
button.list-group-item-success .list-group-item-heading {
  color: inherit;
}
a.list-group-item-success:hover,
button.list-group-item-success:hover,
a.list-group-item-success:focus,
button.list-group-item-success:focus {
  color: #3c763d;
  background-color: #d0e9c6;
}
a.list-group-item-success.active,
button.list-group-item-success.active,
a.list-group-item-success.active:hover,
button.list-group-item-success.active:hover,
a.list-group-item-success.active:focus,
button.list-group-item-success.active:focus {
  color: #fff;
  background-color: #3c763d;
  border-color: #3c763d;
}
.list-group-item-info {
  color: #31708f;
  background-color: #d9edf7;
}
a.list-group-item-info,
button.list-group-item-info {
  color: #31708f;
}
a.list-group-item-info .list-group-item-heading,
button.list-group-item-info .list-group-item-heading {
  color: inherit;
}
a.list-group-item-info:hover,
button.list-group-item-info:hover,
a.list-group-item-info:focus,
button.list-group-item-info:focus {
  color: #31708f;
  background-color: #c4e3f3;
}
a.list-group-item-info.active,
button.list-group-item-info.active,
a.list-group-item-info.active:hover,
button.list-group-item-info.active:hover,
a.list-group-item-info.active:focus,
button.list-group-item-info.active:focus {
  color: #fff;
  background-color: #31708f;
  border-color: #31708f;
}
.list-group-item-warning {
  color: #8a6d3b;
  background-color: #fcf8e3;
}
a.list-group-item-warning,
button.list-group-item-warning {
  color: #8a6d3b;
}
a.list-group-item-warning .list-group-item-heading,
button.list-group-item-warning .list-group-item-heading {
  color: inherit;
}
a.list-group-item-warning:hover,
button.list-group-item-warning:hover,
a.list-group-item-warning:focus,
button.list-group-item-warning:focus {
  color: #8a6d3b;
  background-color: #faf2cc;
}
a.list-group-item-warning.active,
button.list-group-item-warning.active,
a.list-group-item-warning.active:hover,
button.list-group-item-warning.active:hover,
a.list-group-item-warning.active:focus,
button.list-group-item-warning.active:focus {
  color: #fff;
  background-color: #8a6d3b;
  border-color: #8a6d3b;
}
.list-group-item-danger {
  color: #a94442;
  background-color: #f2dede;
}
a.list-group-item-danger,
button.list-group-item-danger {
  color: #a94442;
}
a.list-group-item-danger .list-group-item-heading,
button.list-group-item-danger .list-group-item-heading {
  color: inherit;
}
a.list-group-item-danger:hover,
button.list-group-item-danger:hover,
a.list-group-item-danger:focus,
button.list-group-item-danger:focus {
  color: #a94442;
  background-color: #ebcccc;
}
a.list-group-item-danger.active,
button.list-group-item-danger.active,
a.list-group-item-danger.active:hover,
button.list-group-item-danger.active:hover,
a.list-group-item-danger.active:focus,
button.list-group-item-danger.active:focus {
  color: #fff;
  background-color: #a94442;
  border-color: #a94442;
}
.list-group-item-heading {
  margin-top: 0;
  margin-bottom: 5px;
}
.list-group-item-text {
  margin-bottom: 0;
  line-height: 1.3;
}
.panel {
  margin-bottom: 18px;
  background-color: #fff;
  border: 1px solid transparent;
  border-radius: 2px;
  -webkit-box-shadow: 0 1px 1px rgba(0, 0, 0, 0.05);
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.05);
}
.panel-body {
  padding: 15px;
}
.panel-heading {
  padding: 10px 15px;
  border-bottom: 1px solid transparent;
  border-top-right-radius: 1px;
  border-top-left-radius: 1px;
}
.panel-heading > .dropdown .dropdown-toggle {
  color: inherit;
}
.panel-title {
  margin-top: 0;
  margin-bottom: 0;
  font-size: 15px;
  color: inherit;
}
.panel-title > a,
.panel-title > small,
.panel-title > .small,
.panel-title > small > a,
.panel-title > .small > a {
  color: inherit;
}
.panel-footer {
  padding: 10px 15px;
  background-color: #f5f5f5;
  border-top: 1px solid #ddd;
  border-bottom-right-radius: 1px;
  border-bottom-left-radius: 1px;
}
.panel > .list-group,
.panel > .panel-collapse > .list-group {
  margin-bottom: 0;
}
.panel > .list-group .list-group-item,
.panel > .panel-collapse > .list-group .list-group-item {
  border-width: 1px 0;
  border-radius: 0;
}
.panel > .list-group:first-child .list-group-item:first-child,
.panel > .panel-collapse > .list-group:first-child .list-group-item:first-child {
  border-top: 0;
  border-top-right-radius: 1px;
  border-top-left-radius: 1px;
}
.panel > .list-group:last-child .list-group-item:last-child,
.panel > .panel-collapse > .list-group:last-child .list-group-item:last-child {
  border-bottom: 0;
  border-bottom-right-radius: 1px;
  border-bottom-left-radius: 1px;
}
.panel > .panel-heading + .panel-collapse > .list-group .list-group-item:first-child {
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.panel-heading + .list-group .list-group-item:first-child {
  border-top-width: 0;
}
.list-group + .panel-footer {
  border-top-width: 0;
}
.panel > .table,
.panel > .table-responsive > .table,
.panel > .panel-collapse > .table {
  margin-bottom: 0;
}
.panel > .table caption,
.panel > .table-responsive > .table caption,
.panel > .panel-collapse > .table caption {
  padding-left: 15px;
  padding-right: 15px;
}
.panel > .table:first-child,
.panel > .table-responsive:first-child > .table:first-child {
  border-top-right-radius: 1px;
  border-top-left-radius: 1px;
}
.panel > .table:first-child > thead:first-child > tr:first-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child,
.panel > .table:first-child > tbody:first-child > tr:first-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child {
  border-top-left-radius: 1px;
  border-top-right-radius: 1px;
}
.panel > .table:first-child > thead:first-child > tr:first-child td:first-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child td:first-child,
.panel > .table:first-child > tbody:first-child > tr:first-child td:first-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child td:first-child,
.panel > .table:first-child > thead:first-child > tr:first-child th:first-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child th:first-child,
.panel > .table:first-child > tbody:first-child > tr:first-child th:first-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child th:first-child {
  border-top-left-radius: 1px;
}
.panel > .table:first-child > thead:first-child > tr:first-child td:last-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child td:last-child,
.panel > .table:first-child > tbody:first-child > tr:first-child td:last-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child td:last-child,
.panel > .table:first-child > thead:first-child > tr:first-child th:last-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child th:last-child,
.panel > .table:first-child > tbody:first-child > tr:first-child th:last-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child th:last-child {
  border-top-right-radius: 1px;
}
.panel > .table:last-child,
.panel > .table-responsive:last-child > .table:last-child {
  border-bottom-right-radius: 1px;
  border-bottom-left-radius: 1px;
}
.panel > .table:last-child > tbody:last-child > tr:last-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child {
  border-bottom-left-radius: 1px;
  border-bottom-right-radius: 1px;
}
.panel > .table:last-child > tbody:last-child > tr:last-child td:first-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child td:first-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child td:first-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child td:first-child,
.panel > .table:last-child > tbody:last-child > tr:last-child th:first-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child th:first-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child th:first-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child th:first-child {
  border-bottom-left-radius: 1px;
}
.panel > .table:last-child > tbody:last-child > tr:last-child td:last-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child td:last-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child td:last-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child td:last-child,
.panel > .table:last-child > tbody:last-child > tr:last-child th:last-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child th:last-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child th:last-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child th:last-child {
  border-bottom-right-radius: 1px;
}
.panel > .panel-body + .table,
.panel > .panel-body + .table-responsive,
.panel > .table + .panel-body,
.panel > .table-responsive + .panel-body {
  border-top: 1px solid #ddd;
}
.panel > .table > tbody:first-child > tr:first-child th,
.panel > .table > tbody:first-child > tr:first-child td {
  border-top: 0;
}
.panel > .table-bordered,
.panel > .table-responsive > .table-bordered {
  border: 0;
}
.panel > .table-bordered > thead > tr > th:first-child,
.panel > .table-responsive > .table-bordered > thead > tr > th:first-child,
.panel > .table-bordered > tbody > tr > th:first-child,
.panel > .table-responsive > .table-bordered > tbody > tr > th:first-child,
.panel > .table-bordered > tfoot > tr > th:first-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > th:first-child,
.panel > .table-bordered > thead > tr > td:first-child,
.panel > .table-responsive > .table-bordered > thead > tr > td:first-child,
.panel > .table-bordered > tbody > tr > td:first-child,
.panel > .table-responsive > .table-bordered > tbody > tr > td:first-child,
.panel > .table-bordered > tfoot > tr > td:first-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > td:first-child {
  border-left: 0;
}
.panel > .table-bordered > thead > tr > th:last-child,
.panel > .table-responsive > .table-bordered > thead > tr > th:last-child,
.panel > .table-bordered > tbody > tr > th:last-child,
.panel > .table-responsive > .table-bordered > tbody > tr > th:last-child,
.panel > .table-bordered > tfoot > tr > th:last-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > th:last-child,
.panel > .table-bordered > thead > tr > td:last-child,
.panel > .table-responsive > .table-bordered > thead > tr > td:last-child,
.panel > .table-bordered > tbody > tr > td:last-child,
.panel > .table-responsive > .table-bordered > tbody > tr > td:last-child,
.panel > .table-bordered > tfoot > tr > td:last-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > td:last-child {
  border-right: 0;
}
.panel > .table-bordered > thead > tr:first-child > td,
.panel > .table-responsive > .table-bordered > thead > tr:first-child > td,
.panel > .table-bordered > tbody > tr:first-child > td,
.panel > .table-responsive > .table-bordered > tbody > tr:first-child > td,
.panel > .table-bordered > thead > tr:first-child > th,
.panel > .table-responsive > .table-bordered > thead > tr:first-child > th,
.panel > .table-bordered > tbody > tr:first-child > th,
.panel > .table-responsive > .table-bordered > tbody > tr:first-child > th {
  border-bottom: 0;
}
.panel > .table-bordered > tbody > tr:last-child > td,
.panel > .table-responsive > .table-bordered > tbody > tr:last-child > td,
.panel > .table-bordered > tfoot > tr:last-child > td,
.panel > .table-responsive > .table-bordered > tfoot > tr:last-child > td,
.panel > .table-bordered > tbody > tr:last-child > th,
.panel > .table-responsive > .table-bordered > tbody > tr:last-child > th,
.panel > .table-bordered > tfoot > tr:last-child > th,
.panel > .table-responsive > .table-bordered > tfoot > tr:last-child > th {
  border-bottom: 0;
}
.panel > .table-responsive {
  border: 0;
  margin-bottom: 0;
}
.panel-group {
  margin-bottom: 18px;
}
.panel-group .panel {
  margin-bottom: 0;
  border-radius: 2px;
}
.panel-group .panel + .panel {
  margin-top: 5px;
}
.panel-group .panel-heading {
  border-bottom: 0;
}
.panel-group .panel-heading + .panel-collapse > .panel-body,
.panel-group .panel-heading + .panel-collapse > .list-group {
  border-top: 1px solid #ddd;
}
.panel-group .panel-footer {
  border-top: 0;
}
.panel-group .panel-footer + .panel-collapse .panel-body {
  border-bottom: 1px solid #ddd;
}
.panel-default {
  border-color: #ddd;
}
.panel-default > .panel-heading {
  color: #333333;
  background-color: #f5f5f5;
  border-color: #ddd;
}
.panel-default > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #ddd;
}
.panel-default > .panel-heading .badge {
  color: #f5f5f5;
  background-color: #333333;
}
.panel-default > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #ddd;
}
.panel-primary {
  border-color: #337ab7;
}
.panel-primary > .panel-heading {
  color: #fff;
  background-color: #337ab7;
  border-color: #337ab7;
}
.panel-primary > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #337ab7;
}
.panel-primary > .panel-heading .badge {
  color: #337ab7;
  background-color: #fff;
}
.panel-primary > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #337ab7;
}
.panel-success {
  border-color: #d6e9c6;
}
.panel-success > .panel-heading {
  color: #3c763d;
  background-color: #dff0d8;
  border-color: #d6e9c6;
}
.panel-success > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #d6e9c6;
}
.panel-success > .panel-heading .badge {
  color: #dff0d8;
  background-color: #3c763d;
}
.panel-success > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #d6e9c6;
}
.panel-info {
  border-color: #bce8f1;
}
.panel-info > .panel-heading {
  color: #31708f;
  background-color: #d9edf7;
  border-color: #bce8f1;
}
.panel-info > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #bce8f1;
}
.panel-info > .panel-heading .badge {
  color: #d9edf7;
  background-color: #31708f;
}
.panel-info > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #bce8f1;
}
.panel-warning {
  border-color: #faebcc;
}
.panel-warning > .panel-heading {
  color: #8a6d3b;
  background-color: #fcf8e3;
  border-color: #faebcc;
}
.panel-warning > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #faebcc;
}
.panel-warning > .panel-heading .badge {
  color: #fcf8e3;
  background-color: #8a6d3b;
}
.panel-warning > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #faebcc;
}
.panel-danger {
  border-color: #ebccd1;
}
.panel-danger > .panel-heading {
  color: #a94442;
  background-color: #f2dede;
  border-color: #ebccd1;
}
.panel-danger > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #ebccd1;
}
.panel-danger > .panel-heading .badge {
  color: #f2dede;
  background-color: #a94442;
}
.panel-danger > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #ebccd1;
}
.embed-responsive {
  position: relative;
  display: block;
  height: 0;
  padding: 0;
  overflow: hidden;
}
.embed-responsive .embed-responsive-item,
.embed-responsive iframe,
.embed-responsive embed,
.embed-responsive object,
.embed-responsive video {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  height: 100%;
  width: 100%;
  border: 0;
}
.embed-responsive-16by9 {
  padding-bottom: 56.25%;
}
.embed-responsive-4by3 {
  padding-bottom: 75%;
}
.well {
  min-height: 20px;
  padding: 19px;
  margin-bottom: 20px;
  background-color: #f5f5f5;
  border: 1px solid #e3e3e3;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.05);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.05);
}
.well blockquote {
  border-color: #ddd;
  border-color: rgba(0, 0, 0, 0.15);
}
.well-lg {
  padding: 24px;
  border-radius: 3px;
}
.well-sm {
  padding: 9px;
  border-radius: 1px;
}
.close {
  float: right;
  font-size: 19.5px;
  font-weight: bold;
  line-height: 1;
  color: #000;
  text-shadow: 0 1px 0 #fff;
  opacity: 0.2;
  filter: alpha(opacity=20);
}
.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  cursor: pointer;
  opacity: 0.5;
  filter: alpha(opacity=50);
}
button.close {
  padding: 0;
  cursor: pointer;
  background: transparent;
  border: 0;
  -webkit-appearance: none;
}
.modal-open {
  overflow: hidden;
}
.modal {
  display: none;
  overflow: hidden;
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1050;
  -webkit-overflow-scrolling: touch;
  outline: 0;
}
.modal.fade .modal-dialog {
  -webkit-transform: translate(0, -25%);
  -ms-transform: translate(0, -25%);
  -o-transform: translate(0, -25%);
  transform: translate(0, -25%);
  -webkit-transition: -webkit-transform 0.3s ease-out;
  -moz-transition: -moz-transform 0.3s ease-out;
  -o-transition: -o-transform 0.3s ease-out;
  transition: transform 0.3s ease-out;
}
.modal.in .modal-dialog {
  -webkit-transform: translate(0, 0);
  -ms-transform: translate(0, 0);
  -o-transform: translate(0, 0);
  transform: translate(0, 0);
}
.modal-open .modal {
  overflow-x: hidden;
  overflow-y: auto;
}
.modal-dialog {
  position: relative;
  width: auto;
  margin: 10px;
}
.modal-content {
  position: relative;
  background-color: #fff;
  border: 1px solid #999;
  border: 1px solid rgba(0, 0, 0, 0.2);
  border-radius: 3px;
  -webkit-box-shadow: 0 3px 9px rgba(0, 0, 0, 0.5);
  box-shadow: 0 3px 9px rgba(0, 0, 0, 0.5);
  background-clip: padding-box;
  outline: 0;
}
.modal-backdrop {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1040;
  background-color: #000;
}
.modal-backdrop.fade {
  opacity: 0;
  filter: alpha(opacity=0);
}
.modal-backdrop.in {
  opacity: 0.5;
  filter: alpha(opacity=50);
}
.modal-header {
  padding: 15px;
  border-bottom: 1px solid #e5e5e5;
}
.modal-header .close {
  margin-top: -2px;
}
.modal-title {
  margin: 0;
  line-height: 1.42857143;
}
.modal-body {
  position: relative;
  padding: 15px;
}
.modal-footer {
  padding: 15px;
  text-align: right;
  border-top: 1px solid #e5e5e5;
}
.modal-footer .btn + .btn {
  margin-left: 5px;
  margin-bottom: 0;
}
.modal-footer .btn-group .btn + .btn {
  margin-left: -1px;
}
.modal-footer .btn-block + .btn-block {
  margin-left: 0;
}
.modal-scrollbar-measure {
  position: absolute;
  top: -9999px;
  width: 50px;
  height: 50px;
  overflow: scroll;
}
@media (min-width: 768px) {
  .modal-dialog {
    width: 600px;
    margin: 30px auto;
  }
  .modal-content {
    -webkit-box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
  }
  .modal-sm {
    width: 300px;
  }
}
@media (min-width: 992px) {
  .modal-lg {
    width: 900px;
  }
}
.tooltip {
  position: absolute;
  z-index: 1070;
  display: block;
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-style: normal;
  font-weight: normal;
  letter-spacing: normal;
  line-break: auto;
  line-height: 1.42857143;
  text-align: left;
  text-align: start;
  text-decoration: none;
  text-shadow: none;
  text-transform: none;
  white-space: normal;
  word-break: normal;
  word-spacing: normal;
  word-wrap: normal;
  font-size: 12px;
  opacity: 0;
  filter: alpha(opacity=0);
}
.tooltip.in {
  opacity: 0.9;
  filter: alpha(opacity=90);
}
.tooltip.top {
  margin-top: -3px;
  padding: 5px 0;
}
.tooltip.right {
  margin-left: 3px;
  padding: 0 5px;
}
.tooltip.bottom {
  margin-top: 3px;
  padding: 5px 0;
}
.tooltip.left {
  margin-left: -3px;
  padding: 0 5px;
}
.tooltip-inner {
  max-width: 200px;
  padding: 3px 8px;
  color: #fff;
  text-align: center;
  background-color: #000;
  border-radius: 2px;
}
.tooltip-arrow {
  position: absolute;
  width: 0;
  height: 0;
  border-color: transparent;
  border-style: solid;
}
.tooltip.top .tooltip-arrow {
  bottom: 0;
  left: 50%;
  margin-left: -5px;
  border-width: 5px 5px 0;
  border-top-color: #000;
}
.tooltip.top-left .tooltip-arrow {
  bottom: 0;
  right: 5px;
  margin-bottom: -5px;
  border-width: 5px 5px 0;
  border-top-color: #000;
}
.tooltip.top-right .tooltip-arrow {
  bottom: 0;
  left: 5px;
  margin-bottom: -5px;
  border-width: 5px 5px 0;
  border-top-color: #000;
}
.tooltip.right .tooltip-arrow {
  top: 50%;
  left: 0;
  margin-top: -5px;
  border-width: 5px 5px 5px 0;
  border-right-color: #000;
}
.tooltip.left .tooltip-arrow {
  top: 50%;
  right: 0;
  margin-top: -5px;
  border-width: 5px 0 5px 5px;
  border-left-color: #000;
}
.tooltip.bottom .tooltip-arrow {
  top: 0;
  left: 50%;
  margin-left: -5px;
  border-width: 0 5px 5px;
  border-bottom-color: #000;
}
.tooltip.bottom-left .tooltip-arrow {
  top: 0;
  right: 5px;
  margin-top: -5px;
  border-width: 0 5px 5px;
  border-bottom-color: #000;
}
.tooltip.bottom-right .tooltip-arrow {
  top: 0;
  left: 5px;
  margin-top: -5px;
  border-width: 0 5px 5px;
  border-bottom-color: #000;
}
.popover {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 1060;
  display: none;
  max-width: 276px;
  padding: 1px;
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-style: normal;
  font-weight: normal;
  letter-spacing: normal;
  line-break: auto;
  line-height: 1.42857143;
  text-align: left;
  text-align: start;
  text-decoration: none;
  text-shadow: none;
  text-transform: none;
  white-space: normal;
  word-break: normal;
  word-spacing: normal;
  word-wrap: normal;
  font-size: 13px;
  background-color: #fff;
  background-clip: padding-box;
  border: 1px solid #ccc;
  border: 1px solid rgba(0, 0, 0, 0.2);
  border-radius: 3px;
  -webkit-box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
}
.popover.top {
  margin-top: -10px;
}
.popover.right {
  margin-left: 10px;
}
.popover.bottom {
  margin-top: 10px;
}
.popover.left {
  margin-left: -10px;
}
.popover-title {
  margin: 0;
  padding: 8px 14px;
  font-size: 13px;
  background-color: #f7f7f7;
  border-bottom: 1px solid #ebebeb;
  border-radius: 2px 2px 0 0;
}
.popover-content {
  padding: 9px 14px;
}
.popover > .arrow,
.popover > .arrow:after {
  position: absolute;
  display: block;
  width: 0;
  height: 0;
  border-color: transparent;
  border-style: solid;
}
.popover > .arrow {
  border-width: 11px;
}
.popover > .arrow:after {
  border-width: 10px;
  content: "";
}
.popover.top > .arrow {
  left: 50%;
  margin-left: -11px;
  border-bottom-width: 0;
  border-top-color: #999999;
  border-top-color: rgba(0, 0, 0, 0.25);
  bottom: -11px;
}
.popover.top > .arrow:after {
  content: " ";
  bottom: 1px;
  margin-left: -10px;
  border-bottom-width: 0;
  border-top-color: #fff;
}
.popover.right > .arrow {
  top: 50%;
  left: -11px;
  margin-top: -11px;
  border-left-width: 0;
  border-right-color: #999999;
  border-right-color: rgba(0, 0, 0, 0.25);
}
.popover.right > .arrow:after {
  content: " ";
  left: 1px;
  bottom: -10px;
  border-left-width: 0;
  border-right-color: #fff;
}
.popover.bottom > .arrow {
  left: 50%;
  margin-left: -11px;
  border-top-width: 0;
  border-bottom-color: #999999;
  border-bottom-color: rgba(0, 0, 0, 0.25);
  top: -11px;
}
.popover.bottom > .arrow:after {
  content: " ";
  top: 1px;
  margin-left: -10px;
  border-top-width: 0;
  border-bottom-color: #fff;
}
.popover.left > .arrow {
  top: 50%;
  right: -11px;
  margin-top: -11px;
  border-right-width: 0;
  border-left-color: #999999;
  border-left-color: rgba(0, 0, 0, 0.25);
}
.popover.left > .arrow:after {
  content: " ";
  right: 1px;
  border-right-width: 0;
  border-left-color: #fff;
  bottom: -10px;
}
.carousel {
  position: relative;
}
.carousel-inner {
  position: relative;
  overflow: hidden;
  width: 100%;
}
.carousel-inner > .item {
  display: none;
  position: relative;
  -webkit-transition: 0.6s ease-in-out left;
  -o-transition: 0.6s ease-in-out left;
  transition: 0.6s ease-in-out left;
}
.carousel-inner > .item > img,
.carousel-inner > .item > a > img {
  line-height: 1;
}
@media all and (transform-3d), (-webkit-transform-3d) {
  .carousel-inner > .item {
    -webkit-transition: -webkit-transform 0.6s ease-in-out;
    -moz-transition: -moz-transform 0.6s ease-in-out;
    -o-transition: -o-transform 0.6s ease-in-out;
    transition: transform 0.6s ease-in-out;
    -webkit-backface-visibility: hidden;
    -moz-backface-visibility: hidden;
    backface-visibility: hidden;
    -webkit-perspective: 1000px;
    -moz-perspective: 1000px;
    perspective: 1000px;
  }
  .carousel-inner > .item.next,
  .carousel-inner > .item.active.right {
    -webkit-transform: translate3d(100%, 0, 0);
    transform: translate3d(100%, 0, 0);
    left: 0;
  }
  .carousel-inner > .item.prev,
  .carousel-inner > .item.active.left {
    -webkit-transform: translate3d(-100%, 0, 0);
    transform: translate3d(-100%, 0, 0);
    left: 0;
  }
  .carousel-inner > .item.next.left,
  .carousel-inner > .item.prev.right,
  .carousel-inner > .item.active {
    -webkit-transform: translate3d(0, 0, 0);
    transform: translate3d(0, 0, 0);
    left: 0;
  }
}
.carousel-inner > .active,
.carousel-inner > .next,
.carousel-inner > .prev {
  display: block;
}
.carousel-inner > .active {
  left: 0;
}
.carousel-inner > .next,
.carousel-inner > .prev {
  position: absolute;
  top: 0;
  width: 100%;
}
.carousel-inner > .next {
  left: 100%;
}
.carousel-inner > .prev {
  left: -100%;
}
.carousel-inner > .next.left,
.carousel-inner > .prev.right {
  left: 0;
}
.carousel-inner > .active.left {
  left: -100%;
}
.carousel-inner > .active.right {
  left: 100%;
}
.carousel-control {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  width: 15%;
  opacity: 0.5;
  filter: alpha(opacity=50);
  font-size: 20px;
  color: #fff;
  text-align: center;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.6);
  background-color: rgba(0, 0, 0, 0);
}
.carousel-control.left {
  background-image: -webkit-linear-gradient(left, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.0001) 100%);
  background-image: -o-linear-gradient(left, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.0001) 100%);
  background-image: linear-gradient(to right, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.0001) 100%);
  background-repeat: repeat-x;
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#80000000', endColorstr='#00000000', GradientType=1);
}
.carousel-control.right {
  left: auto;
  right: 0;
  background-image: -webkit-linear-gradient(left, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.5) 100%);
  background-image: -o-linear-gradient(left, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.5) 100%);
  background-image: linear-gradient(to right, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.5) 100%);
  background-repeat: repeat-x;
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#00000000', endColorstr='#80000000', GradientType=1);
}
.carousel-control:hover,
.carousel-control:focus {
  outline: 0;
  color: #fff;
  text-decoration: none;
  opacity: 0.9;
  filter: alpha(opacity=90);
}
.carousel-control .icon-prev,
.carousel-control .icon-next,
.carousel-control .glyphicon-chevron-left,
.carousel-control .glyphicon-chevron-right {
  position: absolute;
  top: 50%;
  margin-top: -10px;
  z-index: 5;
  display: inline-block;
}
.carousel-control .icon-prev,
.carousel-control .glyphicon-chevron-left {
  left: 50%;
  margin-left: -10px;
}
.carousel-control .icon-next,
.carousel-control .glyphicon-chevron-right {
  right: 50%;
  margin-right: -10px;
}
.carousel-control .icon-prev,
.carousel-control .icon-next {
  width: 20px;
  height: 20px;
  line-height: 1;
  font-family: serif;
}
.carousel-control .icon-prev:before {
  content: '\2039';
}
.carousel-control .icon-next:before {
  content: '\203a';
}
.carousel-indicators {
  position: absolute;
  bottom: 10px;
  left: 50%;
  z-index: 15;
  width: 60%;
  margin-left: -30%;
  padding-left: 0;
  list-style: none;
  text-align: center;
}
.carousel-indicators li {
  display: inline-block;
  width: 10px;
  height: 10px;
  margin: 1px;
  text-indent: -999px;
  border: 1px solid #fff;
  border-radius: 10px;
  cursor: pointer;
  background-color: #000 \9;
  background-color: rgba(0, 0, 0, 0);
}
.carousel-indicators .active {
  margin: 0;
  width: 12px;
  height: 12px;
  background-color: #fff;
}
.carousel-caption {
  position: absolute;
  left: 15%;
  right: 15%;
  bottom: 20px;
  z-index: 10;
  padding-top: 20px;
  padding-bottom: 20px;
  color: #fff;
  text-align: center;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.6);
}
.carousel-caption .btn {
  text-shadow: none;
}
@media screen and (min-width: 768px) {
  .carousel-control .glyphicon-chevron-left,
  .carousel-control .glyphicon-chevron-right,
  .carousel-control .icon-prev,
  .carousel-control .icon-next {
    width: 30px;
    height: 30px;
    margin-top: -10px;
    font-size: 30px;
  }
  .carousel-control .glyphicon-chevron-left,
  .carousel-control .icon-prev {
    margin-left: -10px;
  }
  .carousel-control .glyphicon-chevron-right,
  .carousel-control .icon-next {
    margin-right: -10px;
  }
  .carousel-caption {
    left: 20%;
    right: 20%;
    padding-bottom: 30px;
  }
  .carousel-indicators {
    bottom: 20px;
  }
}
.clearfix:before,
.clearfix:after,
.dl-horizontal dd:before,
.dl-horizontal dd:after,
.container:before,
.container:after,
.container-fluid:before,
.container-fluid:after,
.row:before,
.row:after,
.form-horizontal .form-group:before,
.form-horizontal .form-group:after,
.btn-toolbar:before,
.btn-toolbar:after,
.btn-group-vertical > .btn-group:before,
.btn-group-vertical > .btn-group:after,
.nav:before,
.nav:after,
.navbar:before,
.navbar:after,
.navbar-header:before,
.navbar-header:after,
.navbar-collapse:before,
.navbar-collapse:after,
.pager:before,
.pager:after,
.panel-body:before,
.panel-body:after,
.modal-header:before,
.modal-header:after,
.modal-footer:before,
.modal-footer:after,
.item_buttons:before,
.item_buttons:after {
  content: " ";
  display: table;
}
.clearfix:after,
.dl-horizontal dd:after,
.container:after,
.container-fluid:after,
.row:after,
.form-horizontal .form-group:after,
.btn-toolbar:after,
.btn-group-vertical > .btn-group:after,
.nav:after,
.navbar:after,
.navbar-header:after,
.navbar-collapse:after,
.pager:after,
.panel-body:after,
.modal-header:after,
.modal-footer:after,
.item_buttons:after {
  clear: both;
}
.center-block {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
.pull-right {
  float: right !important;
}
.pull-left {
  float: left !important;
}
.hide {
  display: none !important;
}
.show {
  display: block !important;
}
.invisible {
  visibility: hidden;
}
.text-hide {
  font: 0/0 a;
  color: transparent;
  text-shadow: none;
  background-color: transparent;
  border: 0;
}
.hidden {
  display: none !important;
}
.affix {
  position: fixed;
}
@-ms-viewport {
  width: device-width;
}
.visible-xs,
.visible-sm,
.visible-md,
.visible-lg {
  display: none !important;
}
.visible-xs-block,
.visible-xs-inline,
.visible-xs-inline-block,
.visible-sm-block,
.visible-sm-inline,
.visible-sm-inline-block,
.visible-md-block,
.visible-md-inline,
.visible-md-inline-block,
.visible-lg-block,
.visible-lg-inline,
.visible-lg-inline-block {
  display: none !important;
}
@media (max-width: 767px) {
  .visible-xs {
    display: block !important;
  }
  table.visible-xs {
    display: table !important;
  }
  tr.visible-xs {
    display: table-row !important;
  }
  th.visible-xs,
  td.visible-xs {
    display: table-cell !important;
  }
}
@media (max-width: 767px) {
  .visible-xs-block {
    display: block !important;
  }
}
@media (max-width: 767px) {
  .visible-xs-inline {
    display: inline !important;
  }
}
@media (max-width: 767px) {
  .visible-xs-inline-block {
    display: inline-block !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm {
    display: block !important;
  }
  table.visible-sm {
    display: table !important;
  }
  tr.visible-sm {
    display: table-row !important;
  }
  th.visible-sm,
  td.visible-sm {
    display: table-cell !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm-block {
    display: block !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm-inline {
    display: inline !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm-inline-block {
    display: inline-block !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md {
    display: block !important;
  }
  table.visible-md {
    display: table !important;
  }
  tr.visible-md {
    display: table-row !important;
  }
  th.visible-md,
  td.visible-md {
    display: table-cell !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md-block {
    display: block !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md-inline {
    display: inline !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md-inline-block {
    display: inline-block !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg {
    display: block !important;
  }
  table.visible-lg {
    display: table !important;
  }
  tr.visible-lg {
    display: table-row !important;
  }
  th.visible-lg,
  td.visible-lg {
    display: table-cell !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg-block {
    display: block !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg-inline {
    display: inline !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg-inline-block {
    display: inline-block !important;
  }
}
@media (max-width: 767px) {
  .hidden-xs {
    display: none !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .hidden-sm {
    display: none !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .hidden-md {
    display: none !important;
  }
}
@media (min-width: 1200px) {
  .hidden-lg {
    display: none !important;
  }
}
.visible-print {
  display: none !important;
}
@media print {
  .visible-print {
    display: block !important;
  }
  table.visible-print {
    display: table !important;
  }
  tr.visible-print {
    display: table-row !important;
  }
  th.visible-print,
  td.visible-print {
    display: table-cell !important;
  }
}
.visible-print-block {
  display: none !important;
}
@media print {
  .visible-print-block {
    display: block !important;
  }
}
.visible-print-inline {
  display: none !important;
}
@media print {
  .visible-print-inline {
    display: inline !important;
  }
}
.visible-print-inline-block {
  display: none !important;
}
@media print {
  .visible-print-inline-block {
    display: inline-block !important;
  }
}
@media print {
  .hidden-print {
    display: none !important;
  }
}
/*!
*
* Font Awesome
*
*/
/*!
 *  Font Awesome 4.2.0 by @davegandy - http://fontawesome.io - @fontawesome
 *  License - http://fontawesome.io/license (Font: SIL OFL 1.1, CSS: MIT License)
 */
/* FONT PATH
 * -------------------------- */
@font-face {
  font-family: 'FontAwesome';
  src: url('../components/font-awesome/fonts/fontawesome-webfont.eot?v=4.2.0');
  src: url('../components/font-awesome/fonts/fontawesome-webfont.eot?#iefix&v=4.2.0') format('embedded-opentype'), url('../components/font-awesome/fonts/fontawesome-webfont.woff?v=4.2.0') format('woff'), url('../components/font-awesome/fonts/fontawesome-webfont.ttf?v=4.2.0') format('truetype'), url('../components/font-awesome/fonts/fontawesome-webfont.svg?v=4.2.0#fontawesomeregular') format('svg');
  font-weight: normal;
  font-style: normal;
}
.fa {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
/* makes the font 33% larger relative to the icon container */
.fa-lg {
  font-size: 1.33333333em;
  line-height: 0.75em;
  vertical-align: -15%;
}
.fa-2x {
  font-size: 2em;
}
.fa-3x {
  font-size: 3em;
}
.fa-4x {
  font-size: 4em;
}
.fa-5x {
  font-size: 5em;
}
.fa-fw {
  width: 1.28571429em;
  text-align: center;
}
.fa-ul {
  padding-left: 0;
  margin-left: 2.14285714em;
  list-style-type: none;
}
.fa-ul > li {
  position: relative;
}
.fa-li {
  position: absolute;
  left: -2.14285714em;
  width: 2.14285714em;
  top: 0.14285714em;
  text-align: center;
}
.fa-li.fa-lg {
  left: -1.85714286em;
}
.fa-border {
  padding: .2em .25em .15em;
  border: solid 0.08em #eee;
  border-radius: .1em;
}
.pull-right {
  float: right;
}
.pull-left {
  float: left;
}
.fa.pull-left {
  margin-right: .3em;
}
.fa.pull-right {
  margin-left: .3em;
}
.fa-spin {
  -webkit-animation: fa-spin 2s infinite linear;
  animation: fa-spin 2s infinite linear;
}
@-webkit-keyframes fa-spin {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(359deg);
    transform: rotate(359deg);
  }
}
@keyframes fa-spin {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(359deg);
    transform: rotate(359deg);
  }
}
.fa-rotate-90 {
  filter: progid:DXImageTransform.Microsoft.BasicImage(rotation=1);
  -webkit-transform: rotate(90deg);
  -ms-transform: rotate(90deg);
  transform: rotate(90deg);
}
.fa-rotate-180 {
  filter: progid:DXImageTransform.Microsoft.BasicImage(rotation=2);
  -webkit-transform: rotate(180deg);
  -ms-transform: rotate(180deg);
  transform: rotate(180deg);
}
.fa-rotate-270 {
  filter: progid:DXImageTransform.Microsoft.BasicImage(rotation=3);
  -webkit-transform: rotate(270deg);
  -ms-transform: rotate(270deg);
  transform: rotate(270deg);
}
.fa-flip-horizontal {
  filter: progid:DXImageTransform.Microsoft.BasicImage(rotation=0, mirror=1);
  -webkit-transform: scale(-1, 1);
  -ms-transform: scale(-1, 1);
  transform: scale(-1, 1);
}
.fa-flip-vertical {
  filter: progid:DXImageTransform.Microsoft.BasicImage(rotation=2, mirror=1);
  -webkit-transform: scale(1, -1);
  -ms-transform: scale(1, -1);
  transform: scale(1, -1);
}
:root .fa-rotate-90,
:root .fa-rotate-180,
:root .fa-rotate-270,
:root .fa-flip-horizontal,
:root .fa-flip-vertical {
  filter: none;
}
.fa-stack {
  position: relative;
  display: inline-block;
  width: 2em;
  height: 2em;
  line-height: 2em;
  vertical-align: middle;
}
.fa-stack-1x,
.fa-stack-2x {
  position: absolute;
  left: 0;
  width: 100%;
  text-align: center;
}
.fa-stack-1x {
  line-height: inherit;
}
.fa-stack-2x {
  font-size: 2em;
}
.fa-inverse {
  color: #fff;
}
/* Font Awesome uses the Unicode Private Use Area (PUA) to ensure screen
   readers do not read off random characters that represent icons */
.fa-glass:before {
  content: "\f000";
}
.fa-music:before {
  content: "\f001";
}
.fa-search:before {
  content: "\f002";
}
.fa-envelope-o:before {
  content: "\f003";
}
.fa-heart:before {
  content: "\f004";
}
.fa-star:before {
  content: "\f005";
}
.fa-star-o:before {
  content: "\f006";
}
.fa-user:before {
  content: "\f007";
}
.fa-film:before {
  content: "\f008";
}
.fa-th-large:before {
  content: "\f009";
}
.fa-th:before {
  content: "\f00a";
}
.fa-th-list:before {
  content: "\f00b";
}
.fa-check:before {
  content: "\f00c";
}
.fa-remove:before,
.fa-close:before,
.fa-times:before {
  content: "\f00d";
}
.fa-search-plus:before {
  content: "\f00e";
}
.fa-search-minus:before {
  content: "\f010";
}
.fa-power-off:before {
  content: "\f011";
}
.fa-signal:before {
  content: "\f012";
}
.fa-gear:before,
.fa-cog:before {
  content: "\f013";
}
.fa-trash-o:before {
  content: "\f014";
}
.fa-home:before {
  content: "\f015";
}
.fa-file-o:before {
  content: "\f016";
}
.fa-clock-o:before {
  content: "\f017";
}
.fa-road:before {
  content: "\f018";
}
.fa-download:before {
  content: "\f019";
}
.fa-arrow-circle-o-down:before {
  content: "\f01a";
}
.fa-arrow-circle-o-up:before {
  content: "\f01b";
}
.fa-inbox:before {
  content: "\f01c";
}
.fa-play-circle-o:before {
  content: "\f01d";
}
.fa-rotate-right:before,
.fa-repeat:before {
  content: "\f01e";
}
.fa-refresh:before {
  content: "\f021";
}
.fa-list-alt:before {
  content: "\f022";
}
.fa-lock:before {
  content: "\f023";
}
.fa-flag:before {
  content: "\f024";
}
.fa-headphones:before {
  content: "\f025";
}
.fa-volume-off:before {
  content: "\f026";
}
.fa-volume-down:before {
  content: "\f027";
}
.fa-volume-up:before {
  content: "\f028";
}
.fa-qrcode:before {
  content: "\f029";
}
.fa-barcode:before {
  content: "\f02a";
}
.fa-tag:before {
  content: "\f02b";
}
.fa-tags:before {
  content: "\f02c";
}
.fa-book:before {
  content: "\f02d";
}
.fa-bookmark:before {
  content: "\f02e";
}
.fa-print:before {
  content: "\f02f";
}
.fa-camera:before {
  content: "\f030";
}
.fa-font:before {
  content: "\f031";
}
.fa-bold:before {
  content: "\f032";
}
.fa-italic:before {
  content: "\f033";
}
.fa-text-height:before {
  content: "\f034";
}
.fa-text-width:before {
  content: "\f035";
}
.fa-align-left:before {
  content: "\f036";
}
.fa-align-center:before {
  content: "\f037";
}
.fa-align-right:before {
  content: "\f038";
}
.fa-align-justify:before {
  content: "\f039";
}
.fa-list:before {
  content: "\f03a";
}
.fa-dedent:before,
.fa-outdent:before {
  content: "\f03b";
}
.fa-indent:before {
  content: "\f03c";
}
.fa-video-camera:before {
  content: "\f03d";
}
.fa-photo:before,
.fa-image:before,
.fa-picture-o:before {
  content: "\f03e";
}
.fa-pencil:before {
  content: "\f040";
}
.fa-map-marker:before {
  content: "\f041";
}
.fa-adjust:before {
  content: "\f042";
}
.fa-tint:before {
  content: "\f043";
}
.fa-edit:before,
.fa-pencil-square-o:before {
  content: "\f044";
}
.fa-share-square-o:before {
  content: "\f045";
}
.fa-check-square-o:before {
  content: "\f046";
}
.fa-arrows:before {
  content: "\f047";
}
.fa-step-backward:before {
  content: "\f048";
}
.fa-fast-backward:before {
  content: "\f049";
}
.fa-backward:before {
  content: "\f04a";
}
.fa-play:before {
  content: "\f04b";
}
.fa-pause:before {
  content: "\f04c";
}
.fa-stop:before {
  content: "\f04d";
}
.fa-forward:before {
  content: "\f04e";
}
.fa-fast-forward:before {
  content: "\f050";
}
.fa-step-forward:before {
  content: "\f051";
}
.fa-eject:before {
  content: "\f052";
}
.fa-chevron-left:before {
  content: "\f053";
}
.fa-chevron-right:before {
  content: "\f054";
}
.fa-plus-circle:before {
  content: "\f055";
}
.fa-minus-circle:before {
  content: "\f056";
}
.fa-times-circle:before {
  content: "\f057";
}
.fa-check-circle:before {
  content: "\f058";
}
.fa-question-circle:before {
  content: "\f059";
}
.fa-info-circle:before {
  content: "\f05a";
}
.fa-crosshairs:before {
  content: "\f05b";
}
.fa-times-circle-o:before {
  content: "\f05c";
}
.fa-check-circle-o:before {
  content: "\f05d";
}
.fa-ban:before {
  content: "\f05e";
}
.fa-arrow-left:before {
  content: "\f060";
}
.fa-arrow-right:before {
  content: "\f061";
}
.fa-arrow-up:before {
  content: "\f062";
}
.fa-arrow-down:before {
  content: "\f063";
}
.fa-mail-forward:before,
.fa-share:before {
  content: "\f064";
}
.fa-expand:before {
  content: "\f065";
}
.fa-compress:before {
  content: "\f066";
}
.fa-plus:before {
  content: "\f067";
}
.fa-minus:before {
  content: "\f068";
}
.fa-asterisk:before {
  content: "\f069";
}
.fa-exclamation-circle:before {
  content: "\f06a";
}
.fa-gift:before {
  content: "\f06b";
}
.fa-leaf:before {
  content: "\f06c";
}
.fa-fire:before {
  content: "\f06d";
}
.fa-eye:before {
  content: "\f06e";
}
.fa-eye-slash:before {
  content: "\f070";
}
.fa-warning:before,
.fa-exclamation-triangle:before {
  content: "\f071";
}
.fa-plane:before {
  content: "\f072";
}
.fa-calendar:before {
  content: "\f073";
}
.fa-random:before {
  content: "\f074";
}
.fa-comment:before {
  content: "\f075";
}
.fa-magnet:before {
  content: "\f076";
}
.fa-chevron-up:before {
  content: "\f077";
}
.fa-chevron-down:before {
  content: "\f078";
}
.fa-retweet:before {
  content: "\f079";
}
.fa-shopping-cart:before {
  content: "\f07a";
}
.fa-folder:before {
  content: "\f07b";
}
.fa-folder-open:before {
  content: "\f07c";
}
.fa-arrows-v:before {
  content: "\f07d";
}
.fa-arrows-h:before {
  content: "\f07e";
}
.fa-bar-chart-o:before,
.fa-bar-chart:before {
  content: "\f080";
}
.fa-twitter-square:before {
  content: "\f081";
}
.fa-facebook-square:before {
  content: "\f082";
}
.fa-camera-retro:before {
  content: "\f083";
}
.fa-key:before {
  content: "\f084";
}
.fa-gears:before,
.fa-cogs:before {
  content: "\f085";
}
.fa-comments:before {
  content: "\f086";
}
.fa-thumbs-o-up:before {
  content: "\f087";
}
.fa-thumbs-o-down:before {
  content: "\f088";
}
.fa-star-half:before {
  content: "\f089";
}
.fa-heart-o:before {
  content: "\f08a";
}
.fa-sign-out:before {
  content: "\f08b";
}
.fa-linkedin-square:before {
  content: "\f08c";
}
.fa-thumb-tack:before {
  content: "\f08d";
}
.fa-external-link:before {
  content: "\f08e";
}
.fa-sign-in:before {
  content: "\f090";
}
.fa-trophy:before {
  content: "\f091";
}
.fa-github-square:before {
  content: "\f092";
}
.fa-upload:before {
  content: "\f093";
}
.fa-lemon-o:before {
  content: "\f094";
}
.fa-phone:before {
  content: "\f095";
}
.fa-square-o:before {
  content: "\f096";
}
.fa-bookmark-o:before {
  content: "\f097";
}
.fa-phone-square:before {
  content: "\f098";
}
.fa-twitter:before {
  content: "\f099";
}
.fa-facebook:before {
  content: "\f09a";
}
.fa-github:before {
  content: "\f09b";
}
.fa-unlock:before {
  content: "\f09c";
}
.fa-credit-card:before {
  content: "\f09d";
}
.fa-rss:before {
  content: "\f09e";
}
.fa-hdd-o:before {
  content: "\f0a0";
}
.fa-bullhorn:before {
  content: "\f0a1";
}
.fa-bell:before {
  content: "\f0f3";
}
.fa-certificate:before {
  content: "\f0a3";
}
.fa-hand-o-right:before {
  content: "\f0a4";
}
.fa-hand-o-left:before {
  content: "\f0a5";
}
.fa-hand-o-up:before {
  content: "\f0a6";
}
.fa-hand-o-down:before {
  content: "\f0a7";
}
.fa-arrow-circle-left:before {
  content: "\f0a8";
}
.fa-arrow-circle-right:before {
  content: "\f0a9";
}
.fa-arrow-circle-up:before {
  content: "\f0aa";
}
.fa-arrow-circle-down:before {
  content: "\f0ab";
}
.fa-globe:before {
  content: "\f0ac";
}
.fa-wrench:before {
  content: "\f0ad";
}
.fa-tasks:before {
  content: "\f0ae";
}
.fa-filter:before {
  content: "\f0b0";
}
.fa-briefcase:before {
  content: "\f0b1";
}
.fa-arrows-alt:before {
  content: "\f0b2";
}
.fa-group:before,
.fa-users:before {
  content: "\f0c0";
}
.fa-chain:before,
.fa-link:before {
  content: "\f0c1";
}
.fa-cloud:before {
  content: "\f0c2";
}
.fa-flask:before {
  content: "\f0c3";
}
.fa-cut:before,
.fa-scissors:before {
  content: "\f0c4";
}
.fa-copy:before,
.fa-files-o:before {
  content: "\f0c5";
}
.fa-paperclip:before {
  content: "\f0c6";
}
.fa-save:before,
.fa-floppy-o:before {
  content: "\f0c7";
}
.fa-square:before {
  content: "\f0c8";
}
.fa-navicon:before,
.fa-reorder:before,
.fa-bars:before {
  content: "\f0c9";
}
.fa-list-ul:before {
  content: "\f0ca";
}
.fa-list-ol:before {
  content: "\f0cb";
}
.fa-strikethrough:before {
  content: "\f0cc";
}
.fa-underline:before {
  content: "\f0cd";
}
.fa-table:before {
  content: "\f0ce";
}
.fa-magic:before {
  content: "\f0d0";
}
.fa-truck:before {
  content: "\f0d1";
}
.fa-pinterest:before {
  content: "\f0d2";
}
.fa-pinterest-square:before {
  content: "\f0d3";
}
.fa-google-plus-square:before {
  content: "\f0d4";
}
.fa-google-plus:before {
  content: "\f0d5";
}
.fa-money:before {
  content: "\f0d6";
}
.fa-caret-down:before {
  content: "\f0d7";
}
.fa-caret-up:before {
  content: "\f0d8";
}
.fa-caret-left:before {
  content: "\f0d9";
}
.fa-caret-right:before {
  content: "\f0da";
}
.fa-columns:before {
  content: "\f0db";
}
.fa-unsorted:before,
.fa-sort:before {
  content: "\f0dc";
}
.fa-sort-down:before,
.fa-sort-desc:before {
  content: "\f0dd";
}
.fa-sort-up:before,
.fa-sort-asc:before {
  content: "\f0de";
}
.fa-envelope:before {
  content: "\f0e0";
}
.fa-linkedin:before {
  content: "\f0e1";
}
.fa-rotate-left:before,
.fa-undo:before {
  content: "\f0e2";
}
.fa-legal:before,
.fa-gavel:before {
  content: "\f0e3";
}
.fa-dashboard:before,
.fa-tachometer:before {
  content: "\f0e4";
}
.fa-comment-o:before {
  content: "\f0e5";
}
.fa-comments-o:before {
  content: "\f0e6";
}
.fa-flash:before,
.fa-bolt:before {
  content: "\f0e7";
}
.fa-sitemap:before {
  content: "\f0e8";
}
.fa-umbrella:before {
  content: "\f0e9";
}
.fa-paste:before,
.fa-clipboard:before {
  content: "\f0ea";
}
.fa-lightbulb-o:before {
  content: "\f0eb";
}
.fa-exchange:before {
  content: "\f0ec";
}
.fa-cloud-download:before {
  content: "\f0ed";
}
.fa-cloud-upload:before {
  content: "\f0ee";
}
.fa-user-md:before {
  content: "\f0f0";
}
.fa-stethoscope:before {
  content: "\f0f1";
}
.fa-suitcase:before {
  content: "\f0f2";
}
.fa-bell-o:before {
  content: "\f0a2";
}
.fa-coffee:before {
  content: "\f0f4";
}
.fa-cutlery:before {
  content: "\f0f5";
}
.fa-file-text-o:before {
  content: "\f0f6";
}
.fa-building-o:before {
  content: "\f0f7";
}
.fa-hospital-o:before {
  content: "\f0f8";
}
.fa-ambulance:before {
  content: "\f0f9";
}
.fa-medkit:before {
  content: "\f0fa";
}
.fa-fighter-jet:before {
  content: "\f0fb";
}
.fa-beer:before {
  content: "\f0fc";
}
.fa-h-square:before {
  content: "\f0fd";
}
.fa-plus-square:before {
  content: "\f0fe";
}
.fa-angle-double-left:before {
  content: "\f100";
}
.fa-angle-double-right:before {
  content: "\f101";
}
.fa-angle-double-up:before {
  content: "\f102";
}
.fa-angle-double-down:before {
  content: "\f103";
}
.fa-angle-left:before {
  content: "\f104";
}
.fa-angle-right:before {
  content: "\f105";
}
.fa-angle-up:before {
  content: "\f106";
}
.fa-angle-down:before {
  content: "\f107";
}
.fa-desktop:before {
  content: "\f108";
}
.fa-laptop:before {
  content: "\f109";
}
.fa-tablet:before {
  content: "\f10a";
}
.fa-mobile-phone:before,
.fa-mobile:before {
  content: "\f10b";
}
.fa-circle-o:before {
  content: "\f10c";
}
.fa-quote-left:before {
  content: "\f10d";
}
.fa-quote-right:before {
  content: "\f10e";
}
.fa-spinner:before {
  content: "\f110";
}
.fa-circle:before {
  content: "\f111";
}
.fa-mail-reply:before,
.fa-reply:before {
  content: "\f112";
}
.fa-github-alt:before {
  content: "\f113";
}
.fa-folder-o:before {
  content: "\f114";
}
.fa-folder-open-o:before {
  content: "\f115";
}
.fa-smile-o:before {
  content: "\f118";
}
.fa-frown-o:before {
  content: "\f119";
}
.fa-meh-o:before {
  content: "\f11a";
}
.fa-gamepad:before {
  content: "\f11b";
}
.fa-keyboard-o:before {
  content: "\f11c";
}
.fa-flag-o:before {
  content: "\f11d";
}
.fa-flag-checkered:before {
  content: "\f11e";
}
.fa-terminal:before {
  content: "\f120";
}
.fa-code:before {
  content: "\f121";
}
.fa-mail-reply-all:before,
.fa-reply-all:before {
  content: "\f122";
}
.fa-star-half-empty:before,
.fa-star-half-full:before,
.fa-star-half-o:before {
  content: "\f123";
}
.fa-location-arrow:before {
  content: "\f124";
}
.fa-crop:before {
  content: "\f125";
}
.fa-code-fork:before {
  content: "\f126";
}
.fa-unlink:before,
.fa-chain-broken:before {
  content: "\f127";
}
.fa-question:before {
  content: "\f128";
}
.fa-info:before {
  content: "\f129";
}
.fa-exclamation:before {
  content: "\f12a";
}
.fa-superscript:before {
  content: "\f12b";
}
.fa-subscript:before {
  content: "\f12c";
}
.fa-eraser:before {
  content: "\f12d";
}
.fa-puzzle-piece:before {
  content: "\f12e";
}
.fa-microphone:before {
  content: "\f130";
}
.fa-microphone-slash:before {
  content: "\f131";
}
.fa-shield:before {
  content: "\f132";
}
.fa-calendar-o:before {
  content: "\f133";
}
.fa-fire-extinguisher:before {
  content: "\f134";
}
.fa-rocket:before {
  content: "\f135";
}
.fa-maxcdn:before {
  content: "\f136";
}
.fa-chevron-circle-left:before {
  content: "\f137";
}
.fa-chevron-circle-right:before {
  content: "\f138";
}
.fa-chevron-circle-up:before {
  content: "\f139";
}
.fa-chevron-circle-down:before {
  content: "\f13a";
}
.fa-html5:before {
  content: "\f13b";
}
.fa-css3:before {
  content: "\f13c";
}
.fa-anchor:before {
  content: "\f13d";
}
.fa-unlock-alt:before {
  content: "\f13e";
}
.fa-bullseye:before {
  content: "\f140";
}
.fa-ellipsis-h:before {
  content: "\f141";
}
.fa-ellipsis-v:before {
  content: "\f142";
}
.fa-rss-square:before {
  content: "\f143";
}
.fa-play-circle:before {
  content: "\f144";
}
.fa-ticket:before {
  content: "\f145";
}
.fa-minus-square:before {
  content: "\f146";
}
.fa-minus-square-o:before {
  content: "\f147";
}
.fa-level-up:before {
  content: "\f148";
}
.fa-level-down:before {
  content: "\f149";
}
.fa-check-square:before {
  content: "\f14a";
}
.fa-pencil-square:before {
  content: "\f14b";
}
.fa-external-link-square:before {
  content: "\f14c";
}
.fa-share-square:before {
  content: "\f14d";
}
.fa-compass:before {
  content: "\f14e";
}
.fa-toggle-down:before,
.fa-caret-square-o-down:before {
  content: "\f150";
}
.fa-toggle-up:before,
.fa-caret-square-o-up:before {
  content: "\f151";
}
.fa-toggle-right:before,
.fa-caret-square-o-right:before {
  content: "\f152";
}
.fa-euro:before,
.fa-eur:before {
  content: "\f153";
}
.fa-gbp:before {
  content: "\f154";
}
.fa-dollar:before,
.fa-usd:before {
  content: "\f155";
}
.fa-rupee:before,
.fa-inr:before {
  content: "\f156";
}
.fa-cny:before,
.fa-rmb:before,
.fa-yen:before,
.fa-jpy:before {
  content: "\f157";
}
.fa-ruble:before,
.fa-rouble:before,
.fa-rub:before {
  content: "\f158";
}
.fa-won:before,
.fa-krw:before {
  content: "\f159";
}
.fa-bitcoin:before,
.fa-btc:before {
  content: "\f15a";
}
.fa-file:before {
  content: "\f15b";
}
.fa-file-text:before {
  content: "\f15c";
}
.fa-sort-alpha-asc:before {
  content: "\f15d";
}
.fa-sort-alpha-desc:before {
  content: "\f15e";
}
.fa-sort-amount-asc:before {
  content: "\f160";
}
.fa-sort-amount-desc:before {
  content: "\f161";
}
.fa-sort-numeric-asc:before {
  content: "\f162";
}
.fa-sort-numeric-desc:before {
  content: "\f163";
}
.fa-thumbs-up:before {
  content: "\f164";
}
.fa-thumbs-down:before {
  content: "\f165";
}
.fa-youtube-square:before {
  content: "\f166";
}
.fa-youtube:before {
  content: "\f167";
}
.fa-xing:before {
  content: "\f168";
}
.fa-xing-square:before {
  content: "\f169";
}
.fa-youtube-play:before {
  content: "\f16a";
}
.fa-dropbox:before {
  content: "\f16b";
}
.fa-stack-overflow:before {
  content: "\f16c";
}
.fa-instagram:before {
  content: "\f16d";
}
.fa-flickr:before {
  content: "\f16e";
}
.fa-adn:before {
  content: "\f170";
}
.fa-bitbucket:before {
  content: "\f171";
}
.fa-bitbucket-square:before {
  content: "\f172";
}
.fa-tumblr:before {
  content: "\f173";
}
.fa-tumblr-square:before {
  content: "\f174";
}
.fa-long-arrow-down:before {
  content: "\f175";
}
.fa-long-arrow-up:before {
  content: "\f176";
}
.fa-long-arrow-left:before {
  content: "\f177";
}
.fa-long-arrow-right:before {
  content: "\f178";
}
.fa-apple:before {
  content: "\f179";
}
.fa-windows:before {
  content: "\f17a";
}
.fa-android:before {
  content: "\f17b";
}
.fa-linux:before {
  content: "\f17c";
}
.fa-dribbble:before {
  content: "\f17d";
}
.fa-skype:before {
  content: "\f17e";
}
.fa-foursquare:before {
  content: "\f180";
}
.fa-trello:before {
  content: "\f181";
}
.fa-female:before {
  content: "\f182";
}
.fa-male:before {
  content: "\f183";
}
.fa-gittip:before {
  content: "\f184";
}
.fa-sun-o:before {
  content: "\f185";
}
.fa-moon-o:before {
  content: "\f186";
}
.fa-archive:before {
  content: "\f187";
}
.fa-bug:before {
  content: "\f188";
}
.fa-vk:before {
  content: "\f189";
}
.fa-weibo:before {
  content: "\f18a";
}
.fa-renren:before {
  content: "\f18b";
}
.fa-pagelines:before {
  content: "\f18c";
}
.fa-stack-exchange:before {
  content: "\f18d";
}
.fa-arrow-circle-o-right:before {
  content: "\f18e";
}
.fa-arrow-circle-o-left:before {
  content: "\f190";
}
.fa-toggle-left:before,
.fa-caret-square-o-left:before {
  content: "\f191";
}
.fa-dot-circle-o:before {
  content: "\f192";
}
.fa-wheelchair:before {
  content: "\f193";
}
.fa-vimeo-square:before {
  content: "\f194";
}
.fa-turkish-lira:before,
.fa-try:before {
  content: "\f195";
}
.fa-plus-square-o:before {
  content: "\f196";
}
.fa-space-shuttle:before {
  content: "\f197";
}
.fa-slack:before {
  content: "\f198";
}
.fa-envelope-square:before {
  content: "\f199";
}
.fa-wordpress:before {
  content: "\f19a";
}
.fa-openid:before {
  content: "\f19b";
}
.fa-institution:before,
.fa-bank:before,
.fa-university:before {
  content: "\f19c";
}
.fa-mortar-board:before,
.fa-graduation-cap:before {
  content: "\f19d";
}
.fa-yahoo:before {
  content: "\f19e";
}
.fa-google:before {
  content: "\f1a0";
}
.fa-reddit:before {
  content: "\f1a1";
}
.fa-reddit-square:before {
  content: "\f1a2";
}
.fa-stumbleupon-circle:before {
  content: "\f1a3";
}
.fa-stumbleupon:before {
  content: "\f1a4";
}
.fa-delicious:before {
  content: "\f1a5";
}
.fa-digg:before {
  content: "\f1a6";
}
.fa-pied-piper:before {
  content: "\f1a7";
}
.fa-pied-piper-alt:before {
  content: "\f1a8";
}
.fa-drupal:before {
  content: "\f1a9";
}
.fa-joomla:before {
  content: "\f1aa";
}
.fa-language:before {
  content: "\f1ab";
}
.fa-fax:before {
  content: "\f1ac";
}
.fa-building:before {
  content: "\f1ad";
}
.fa-child:before {
  content: "\f1ae";
}
.fa-paw:before {
  content: "\f1b0";
}
.fa-spoon:before {
  content: "\f1b1";
}
.fa-cube:before {
  content: "\f1b2";
}
.fa-cubes:before {
  content: "\f1b3";
}
.fa-behance:before {
  content: "\f1b4";
}
.fa-behance-square:before {
  content: "\f1b5";
}
.fa-steam:before {
  content: "\f1b6";
}
.fa-steam-square:before {
  content: "\f1b7";
}
.fa-recycle:before {
  content: "\f1b8";
}
.fa-automobile:before,
.fa-car:before {
  content: "\f1b9";
}
.fa-cab:before,
.fa-taxi:before {
  content: "\f1ba";
}
.fa-tree:before {
  content: "\f1bb";
}
.fa-spotify:before {
  content: "\f1bc";
}
.fa-deviantart:before {
  content: "\f1bd";
}
.fa-soundcloud:before {
  content: "\f1be";
}
.fa-database:before {
  content: "\f1c0";
}
.fa-file-pdf-o:before {
  content: "\f1c1";
}
.fa-file-word-o:before {
  content: "\f1c2";
}
.fa-file-excel-o:before {
  content: "\f1c3";
}
.fa-file-powerpoint-o:before {
  content: "\f1c4";
}
.fa-file-photo-o:before,
.fa-file-picture-o:before,
.fa-file-image-o:before {
  content: "\f1c5";
}
.fa-file-zip-o:before,
.fa-file-archive-o:before {
  content: "\f1c6";
}
.fa-file-sound-o:before,
.fa-file-audio-o:before {
  content: "\f1c7";
}
.fa-file-movie-o:before,
.fa-file-video-o:before {
  content: "\f1c8";
}
.fa-file-code-o:before {
  content: "\f1c9";
}
.fa-vine:before {
  content: "\f1ca";
}
.fa-codepen:before {
  content: "\f1cb";
}
.fa-jsfiddle:before {
  content: "\f1cc";
}
.fa-life-bouy:before,
.fa-life-buoy:before,
.fa-life-saver:before,
.fa-support:before,
.fa-life-ring:before {
  content: "\f1cd";
}
.fa-circle-o-notch:before {
  content: "\f1ce";
}
.fa-ra:before,
.fa-rebel:before {
  content: "\f1d0";
}
.fa-ge:before,
.fa-empire:before {
  content: "\f1d1";
}
.fa-git-square:before {
  content: "\f1d2";
}
.fa-git:before {
  content: "\f1d3";
}
.fa-hacker-news:before {
  content: "\f1d4";
}
.fa-tencent-weibo:before {
  content: "\f1d5";
}
.fa-qq:before {
  content: "\f1d6";
}
.fa-wechat:before,
.fa-weixin:before {
  content: "\f1d7";
}
.fa-send:before,
.fa-paper-plane:before {
  content: "\f1d8";
}
.fa-send-o:before,
.fa-paper-plane-o:before {
  content: "\f1d9";
}
.fa-history:before {
  content: "\f1da";
}
.fa-circle-thin:before {
  content: "\f1db";
}
.fa-header:before {
  content: "\f1dc";
}
.fa-paragraph:before {
  content: "\f1dd";
}
.fa-sliders:before {
  content: "\f1de";
}
.fa-share-alt:before {
  content: "\f1e0";
}
.fa-share-alt-square:before {
  content: "\f1e1";
}
.fa-bomb:before {
  content: "\f1e2";
}
.fa-soccer-ball-o:before,
.fa-futbol-o:before {
  content: "\f1e3";
}
.fa-tty:before {
  content: "\f1e4";
}
.fa-binoculars:before {
  content: "\f1e5";
}
.fa-plug:before {
  content: "\f1e6";
}
.fa-slideshare:before {
  content: "\f1e7";
}
.fa-twitch:before {
  content: "\f1e8";
}
.fa-yelp:before {
  content: "\f1e9";
}
.fa-newspaper-o:before {
  content: "\f1ea";
}
.fa-wifi:before {
  content: "\f1eb";
}
.fa-calculator:before {
  content: "\f1ec";
}
.fa-paypal:before {
  content: "\f1ed";
}
.fa-google-wallet:before {
  content: "\f1ee";
}
.fa-cc-visa:before {
  content: "\f1f0";
}
.fa-cc-mastercard:before {
  content: "\f1f1";
}
.fa-cc-discover:before {
  content: "\f1f2";
}
.fa-cc-amex:before {
  content: "\f1f3";
}
.fa-cc-paypal:before {
  content: "\f1f4";
}
.fa-cc-stripe:before {
  content: "\f1f5";
}
.fa-bell-slash:before {
  content: "\f1f6";
}
.fa-bell-slash-o:before {
  content: "\f1f7";
}
.fa-trash:before {
  content: "\f1f8";
}
.fa-copyright:before {
  content: "\f1f9";
}
.fa-at:before {
  content: "\f1fa";
}
.fa-eyedropper:before {
  content: "\f1fb";
}
.fa-paint-brush:before {
  content: "\f1fc";
}
.fa-birthday-cake:before {
  content: "\f1fd";
}
.fa-area-chart:before {
  content: "\f1fe";
}
.fa-pie-chart:before {
  content: "\f200";
}
.fa-line-chart:before {
  content: "\f201";
}
.fa-lastfm:before {
  content: "\f202";
}
.fa-lastfm-square:before {
  content: "\f203";
}
.fa-toggle-off:before {
  content: "\f204";
}
.fa-toggle-on:before {
  content: "\f205";
}
.fa-bicycle:before {
  content: "\f206";
}
.fa-bus:before {
  content: "\f207";
}
.fa-ioxhost:before {
  content: "\f208";
}
.fa-angellist:before {
  content: "\f209";
}
.fa-cc:before {
  content: "\f20a";
}
.fa-shekel:before,
.fa-sheqel:before,
.fa-ils:before {
  content: "\f20b";
}
.fa-meanpath:before {
  content: "\f20c";
}
/*!
*
* IPython base
*
*/
.modal.fade .modal-dialog {
  -webkit-transform: translate(0, 0);
  -ms-transform: translate(0, 0);
  -o-transform: translate(0, 0);
  transform: translate(0, 0);
}
code {
  color: #000;
}
pre {
  font-size: inherit;
  line-height: inherit;
}
label {
  font-weight: normal;
}
/* Make the page background atleast 100% the height of the view port */
/* Make the page itself atleast 70% the height of the view port */
.border-box-sizing {
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
.corner-all {
  border-radius: 2px;
}
.no-padding {
  padding: 0px;
}
/* Flexible box model classes */
/* Taken from Alex Russell http://infrequently.org/2009/08/css-3-progress/ */
/* This file is a compatability layer.  It allows the usage of flexible box 
model layouts accross multiple browsers, including older browsers.  The newest,
universal implementation of the flexible box model is used when available (see
`Modern browsers` comments below).  Browsers that are known to implement this 
new spec completely include:

    Firefox 28.0+
    Chrome 29.0+
    Internet Explorer 11+ 
    Opera 17.0+

Browsers not listed, including Safari, are supported via the styling under the
`Old browsers` comments below.
*/
.hbox {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
.hbox > * {
  /* Old browsers */
  -webkit-box-flex: 0;
  -moz-box-flex: 0;
  box-flex: 0;
  /* Modern browsers */
  flex: none;
}
.vbox {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
}
.vbox > * {
  /* Old browsers */
  -webkit-box-flex: 0;
  -moz-box-flex: 0;
  box-flex: 0;
  /* Modern browsers */
  flex: none;
}
.hbox.reverse,
.vbox.reverse,
.reverse {
  /* Old browsers */
  -webkit-box-direction: reverse;
  -moz-box-direction: reverse;
  box-direction: reverse;
  /* Modern browsers */
  flex-direction: row-reverse;
}
.hbox.box-flex0,
.vbox.box-flex0,
.box-flex0 {
  /* Old browsers */
  -webkit-box-flex: 0;
  -moz-box-flex: 0;
  box-flex: 0;
  /* Modern browsers */
  flex: none;
  width: auto;
}
.hbox.box-flex1,
.vbox.box-flex1,
.box-flex1 {
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
.hbox.box-flex,
.vbox.box-flex,
.box-flex {
  /* Old browsers */
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
.hbox.box-flex2,
.vbox.box-flex2,
.box-flex2 {
  /* Old browsers */
  -webkit-box-flex: 2;
  -moz-box-flex: 2;
  box-flex: 2;
  /* Modern browsers */
  flex: 2;
}
.box-group1 {
  /*  Deprecated */
  -webkit-box-flex-group: 1;
  -moz-box-flex-group: 1;
  box-flex-group: 1;
}
.box-group2 {
  /* Deprecated */
  -webkit-box-flex-group: 2;
  -moz-box-flex-group: 2;
  box-flex-group: 2;
}
.hbox.start,
.vbox.start,
.start {
  /* Old browsers */
  -webkit-box-pack: start;
  -moz-box-pack: start;
  box-pack: start;
  /* Modern browsers */
  justify-content: flex-start;
}
.hbox.end,
.vbox.end,
.end {
  /* Old browsers */
  -webkit-box-pack: end;
  -moz-box-pack: end;
  box-pack: end;
  /* Modern browsers */
  justify-content: flex-end;
}
.hbox.center,
.vbox.center,
.center {
  /* Old browsers */
  -webkit-box-pack: center;
  -moz-box-pack: center;
  box-pack: center;
  /* Modern browsers */
  justify-content: center;
}
.hbox.baseline,
.vbox.baseline,
.baseline {
  /* Old browsers */
  -webkit-box-pack: baseline;
  -moz-box-pack: baseline;
  box-pack: baseline;
  /* Modern browsers */
  justify-content: baseline;
}
.hbox.stretch,
.vbox.stretch,
.stretch {
  /* Old browsers */
  -webkit-box-pack: stretch;
  -moz-box-pack: stretch;
  box-pack: stretch;
  /* Modern browsers */
  justify-content: stretch;
}
.hbox.align-start,
.vbox.align-start,
.align-start {
  /* Old browsers */
  -webkit-box-align: start;
  -moz-box-align: start;
  box-align: start;
  /* Modern browsers */
  align-items: flex-start;
}
.hbox.align-end,
.vbox.align-end,
.align-end {
  /* Old browsers */
  -webkit-box-align: end;
  -moz-box-align: end;
  box-align: end;
  /* Modern browsers */
  align-items: flex-end;
}
.hbox.align-center,
.vbox.align-center,
.align-center {
  /* Old browsers */
  -webkit-box-align: center;
  -moz-box-align: center;
  box-align: center;
  /* Modern browsers */
  align-items: center;
}
.hbox.align-baseline,
.vbox.align-baseline,
.align-baseline {
  /* Old browsers */
  -webkit-box-align: baseline;
  -moz-box-align: baseline;
  box-align: baseline;
  /* Modern browsers */
  align-items: baseline;
}
.hbox.align-stretch,
.vbox.align-stretch,
.align-stretch {
  /* Old browsers */
  -webkit-box-align: stretch;
  -moz-box-align: stretch;
  box-align: stretch;
  /* Modern browsers */
  align-items: stretch;
}
div.error {
  margin: 2em;
  text-align: center;
}
div.error > h1 {
  font-size: 500%;
  line-height: normal;
}
div.error > p {
  font-size: 200%;
  line-height: normal;
}
div.traceback-wrapper {
  text-align: left;
  max-width: 800px;
  margin: auto;
}
/**
 * Primary styles
 *
 * Author: Jupyter Development Team
 */
body {
  background-color: #fff;
  /* This makes sure that the body covers the entire window and needs to
       be in a different element than the display: box in wrapper below */
  position: absolute;
  left: 0px;
  right: 0px;
  top: 0px;
  bottom: 0px;
  overflow: visible;
}
body > #header {
  /* Initially hidden to prevent FLOUC */
  display: none;
  background-color: #fff;
  /* Display over codemirror */
  position: relative;
  z-index: 100;
}
body > #header #header-container {
  padding-bottom: 5px;
  padding-top: 5px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
body > #header .header-bar {
  width: 100%;
  height: 1px;
  background: #e7e7e7;
  margin-bottom: -1px;
}
@media print {
  body > #header {
    display: none !important;
  }
}
#header-spacer {
  width: 100%;
  visibility: hidden;
}
@media print {
  #header-spacer {
    display: none;
  }
}
#ipython_notebook {
  padding-left: 0px;
  padding-top: 1px;
  padding-bottom: 1px;
}
@media (max-width: 991px) {
  #ipython_notebook {
    margin-left: 10px;
  }
}
[dir="rtl"] #ipython_notebook {
  float: right !important;
}
#noscript {
  width: auto;
  padding-top: 16px;
  padding-bottom: 16px;
  text-align: center;
  font-size: 22px;
  color: red;
  font-weight: bold;
}
#ipython_notebook img {
  height: 28px;
}
#site {
  width: 100%;
  display: none;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  overflow: auto;
}
@media print {
  #site {
    height: auto !important;
  }
}
/* Smaller buttons */
.ui-button .ui-button-text {
  padding: 0.2em 0.8em;
  font-size: 77%;
}
input.ui-button {
  padding: 0.3em 0.9em;
}
span#login_widget {
  float: right;
}
span#login_widget > .button,
#logout {
  color: #333;
  background-color: #fff;
  border-color: #ccc;
}
span#login_widget > .button:focus,
#logout:focus,
span#login_widget > .button.focus,
#logout.focus {
  color: #333;
  background-color: #e6e6e6;
  border-color: #8c8c8c;
}
span#login_widget > .button:hover,
#logout:hover {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
span#login_widget > .button:active,
#logout:active,
span#login_widget > .button.active,
#logout.active,
.open > .dropdown-togglespan#login_widget > .button,
.open > .dropdown-toggle#logout {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
span#login_widget > .button:active:hover,
#logout:active:hover,
span#login_widget > .button.active:hover,
#logout.active:hover,
.open > .dropdown-togglespan#login_widget > .button:hover,
.open > .dropdown-toggle#logout:hover,
span#login_widget > .button:active:focus,
#logout:active:focus,
span#login_widget > .button.active:focus,
#logout.active:focus,
.open > .dropdown-togglespan#login_widget > .button:focus,
.open > .dropdown-toggle#logout:focus,
span#login_widget > .button:active.focus,
#logout:active.focus,
span#login_widget > .button.active.focus,
#logout.active.focus,
.open > .dropdown-togglespan#login_widget > .button.focus,
.open > .dropdown-toggle#logout.focus {
  color: #333;
  background-color: #d4d4d4;
  border-color: #8c8c8c;
}
span#login_widget > .button:active,
#logout:active,
span#login_widget > .button.active,
#logout.active,
.open > .dropdown-togglespan#login_widget > .button,
.open > .dropdown-toggle#logout {
  background-image: none;
}
span#login_widget > .button.disabled:hover,
#logout.disabled:hover,
span#login_widget > .button[disabled]:hover,
#logout[disabled]:hover,
fieldset[disabled] span#login_widget > .button:hover,
fieldset[disabled] #logout:hover,
span#login_widget > .button.disabled:focus,
#logout.disabled:focus,
span#login_widget > .button[disabled]:focus,
#logout[disabled]:focus,
fieldset[disabled] span#login_widget > .button:focus,
fieldset[disabled] #logout:focus,
span#login_widget > .button.disabled.focus,
#logout.disabled.focus,
span#login_widget > .button[disabled].focus,
#logout[disabled].focus,
fieldset[disabled] span#login_widget > .button.focus,
fieldset[disabled] #logout.focus {
  background-color: #fff;
  border-color: #ccc;
}
span#login_widget > .button .badge,
#logout .badge {
  color: #fff;
  background-color: #333;
}
.nav-header {
  text-transform: none;
}
#header > span {
  margin-top: 10px;
}
.modal_stretch .modal-dialog {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  min-height: 80vh;
}
.modal_stretch .modal-dialog .modal-body {
  max-height: calc(100vh - 200px);
  overflow: auto;
  flex: 1;
}
@media (min-width: 768px) {
  .modal .modal-dialog {
    width: 700px;
  }
}
@media (min-width: 768px) {
  select.form-control {
    margin-left: 12px;
    margin-right: 12px;
  }
}
/*!
*
* IPython auth
*
*/
.center-nav {
  display: inline-block;
  margin-bottom: -4px;
}
/*!
*
* IPython tree view
*
*/
/* We need an invisible input field on top of the sentense*/
/* "Drag file onto the list ..." */
.alternate_upload {
  background-color: none;
  display: inline;
}
.alternate_upload.form {
  padding: 0;
  margin: 0;
}
.alternate_upload input.fileinput {
  text-align: center;
  vertical-align: middle;
  display: inline;
  opacity: 0;
  z-index: 2;
  width: 12ex;
  margin-right: -12ex;
}
.alternate_upload .btn-upload {
  height: 22px;
}
/**
 * Primary styles
 *
 * Author: Jupyter Development Team
 */
[dir="rtl"] #tabs li {
  float: right;
}
ul#tabs {
  margin-bottom: 4px;
}
[dir="rtl"] ul#tabs {
  margin-right: 0px;
}
ul#tabs a {
  padding-top: 6px;
  padding-bottom: 4px;
}
ul.breadcrumb a:focus,
ul.breadcrumb a:hover {
  text-decoration: none;
}
ul.breadcrumb i.icon-home {
  font-size: 16px;
  margin-right: 4px;
}
ul.breadcrumb span {
  color: #5e5e5e;
}
.list_toolbar {
  padding: 4px 0 4px 0;
  vertical-align: middle;
}
.list_toolbar .tree-buttons {
  padding-top: 1px;
}
[dir="rtl"] .list_toolbar .tree-buttons {
  float: left !important;
}
[dir="rtl"] .list_toolbar .pull-right {
  padding-top: 1px;
  float: left !important;
}
[dir="rtl"] .list_toolbar .pull-left {
  float: right !important;
}
.dynamic-buttons {
  padding-top: 3px;
  display: inline-block;
}
.list_toolbar [class*="span"] {
  min-height: 24px;
}
.list_header {
  font-weight: bold;
  background-color: #EEE;
}
.list_placeholder {
  font-weight: bold;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 7px;
  padding-right: 7px;
}
.list_container {
  margin-top: 4px;
  margin-bottom: 20px;
  border: 1px solid #ddd;
  border-radius: 2px;
}
.list_container > div {
  border-bottom: 1px solid #ddd;
}
.list_container > div:hover .list-item {
  background-color: red;
}
.list_container > div:last-child {
  border: none;
}
.list_item:hover .list_item {
  background-color: #ddd;
}
.list_item a {
  text-decoration: none;
}
.list_item:hover {
  background-color: #fafafa;
}
.list_header > div,
.list_item > div {
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 7px;
  padding-right: 7px;
  line-height: 22px;
}
.list_header > div input,
.list_item > div input {
  margin-right: 7px;
  margin-left: 14px;
  vertical-align: baseline;
  line-height: 22px;
  position: relative;
  top: -1px;
}
.list_header > div .item_link,
.list_item > div .item_link {
  margin-left: -1px;
  vertical-align: baseline;
  line-height: 22px;
}
.new-file input[type=checkbox] {
  visibility: hidden;
}
.item_name {
  line-height: 22px;
  height: 24px;
}
.item_icon {
  font-size: 14px;
  color: #5e5e5e;
  margin-right: 7px;
  margin-left: 7px;
  line-height: 22px;
  vertical-align: baseline;
}
.item_buttons {
  line-height: 1em;
  margin-left: -5px;
}
.item_buttons .btn,
.item_buttons .btn-group,
.item_buttons .input-group {
  float: left;
}
.item_buttons > .btn,
.item_buttons > .btn-group,
.item_buttons > .input-group {
  margin-left: 5px;
}
.item_buttons .btn {
  min-width: 13ex;
}
.item_buttons .running-indicator {
  padding-top: 4px;
  color: #5cb85c;
}
.item_buttons .kernel-name {
  padding-top: 4px;
  color: #5bc0de;
  margin-right: 7px;
  float: left;
}
.toolbar_info {
  height: 24px;
  line-height: 24px;
}
.list_item input:not([type=checkbox]) {
  padding-top: 3px;
  padding-bottom: 3px;
  height: 22px;
  line-height: 14px;
  margin: 0px;
}
.highlight_text {
  color: blue;
}
#project_name {
  display: inline-block;
  padding-left: 7px;
  margin-left: -2px;
}
#project_name > .breadcrumb {
  padding: 0px;
  margin-bottom: 0px;
  background-color: transparent;
  font-weight: bold;
}
#tree-selector {
  padding-right: 0px;
}
[dir="rtl"] #tree-selector a {
  float: right;
}
#button-select-all {
  min-width: 50px;
}
#select-all {
  margin-left: 7px;
  margin-right: 2px;
}
.menu_icon {
  margin-right: 2px;
}
.tab-content .row {
  margin-left: 0px;
  margin-right: 0px;
}
.folder_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f114";
}
.folder_icon:before.pull-left {
  margin-right: .3em;
}
.folder_icon:before.pull-right {
  margin-left: .3em;
}
.notebook_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f02d";
  position: relative;
  top: -1px;
}
.notebook_icon:before.pull-left {
  margin-right: .3em;
}
.notebook_icon:before.pull-right {
  margin-left: .3em;
}
.running_notebook_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f02d";
  position: relative;
  top: -1px;
  color: #5cb85c;
}
.running_notebook_icon:before.pull-left {
  margin-right: .3em;
}
.running_notebook_icon:before.pull-right {
  margin-left: .3em;
}
.file_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f016";
  position: relative;
  top: -2px;
}
.file_icon:before.pull-left {
  margin-right: .3em;
}
.file_icon:before.pull-right {
  margin-left: .3em;
}
#notebook_toolbar .pull-right {
  padding-top: 0px;
  margin-right: -1px;
}
ul#new-menu {
  left: auto;
  right: 0;
}
[dir="rtl"] #new-menu {
  text-align: right;
}
.kernel-menu-icon {
  padding-right: 12px;
  width: 24px;
  content: "\f096";
}
.kernel-menu-icon:before {
  content: "\f096";
}
.kernel-menu-icon-current:before {
  content: "\f00c";
}
#tab_content {
  padding-top: 20px;
}
#running .panel-group .panel {
  margin-top: 3px;
  margin-bottom: 1em;
}
#running .panel-group .panel .panel-heading {
  background-color: #EEE;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 7px;
  padding-right: 7px;
  line-height: 22px;
}
#running .panel-group .panel .panel-heading a:focus,
#running .panel-group .panel .panel-heading a:hover {
  text-decoration: none;
}
#running .panel-group .panel .panel-body {
  padding: 0px;
}
#running .panel-group .panel .panel-body .list_container {
  margin-top: 0px;
  margin-bottom: 0px;
  border: 0px;
  border-radius: 0px;
}
#running .panel-group .panel .panel-body .list_container .list_item {
  border-bottom: 1px solid #ddd;
}
#running .panel-group .panel .panel-body .list_container .list_item:last-child {
  border-bottom: 0px;
}
[dir="rtl"] #running .col-sm-8 {
  float: right !important;
}
.delete-button {
  display: none;
}
.duplicate-button {
  display: none;
}
.rename-button {
  display: none;
}
.shutdown-button {
  display: none;
}
.dynamic-instructions {
  display: inline-block;
  padding-top: 4px;
}
/*!
*
* IPython text editor webapp
*
*/
.selected-keymap i.fa {
  padding: 0px 5px;
}
.selected-keymap i.fa:before {
  content: "\f00c";
}
#mode-menu {
  overflow: auto;
  max-height: 20em;
}
.edit_app #header {
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
}
.edit_app #menubar .navbar {
  /* Use a negative 1 bottom margin, so the border overlaps the border of the
    header */
  margin-bottom: -1px;
}
.dirty-indicator {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 20px;
}
.dirty-indicator.pull-left {
  margin-right: .3em;
}
.dirty-indicator.pull-right {
  margin-left: .3em;
}
.dirty-indicator-dirty {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 20px;
}
.dirty-indicator-dirty.pull-left {
  margin-right: .3em;
}
.dirty-indicator-dirty.pull-right {
  margin-left: .3em;
}
.dirty-indicator-clean {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 20px;
}
.dirty-indicator-clean.pull-left {
  margin-right: .3em;
}
.dirty-indicator-clean.pull-right {
  margin-left: .3em;
}
.dirty-indicator-clean:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f00c";
}
.dirty-indicator-clean:before.pull-left {
  margin-right: .3em;
}
.dirty-indicator-clean:before.pull-right {
  margin-left: .3em;
}
#filename {
  font-size: 16pt;
  display: table;
  padding: 0px 5px;
}
#current-mode {
  padding-left: 5px;
  padding-right: 5px;
}
#texteditor-backdrop {
  padding-top: 20px;
  padding-bottom: 20px;
}
@media not print {
  #texteditor-backdrop {
    background-color: #EEE;
  }
}
@media print {
  #texteditor-backdrop #texteditor-container .CodeMirror-gutter,
  #texteditor-backdrop #texteditor-container .CodeMirror-gutters {
    background-color: #fff;
  }
}
@media not print {
  #texteditor-backdrop #texteditor-container .CodeMirror-gutter,
  #texteditor-backdrop #texteditor-container .CodeMirror-gutters {
    background-color: #fff;
  }
}
@media not print {
  #texteditor-backdrop #texteditor-container {
    padding: 0px;
    background-color: #fff;
    -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
    box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  }
}
/*!
*
* IPython notebook
*
*/
/* CSS font colors for translated ANSI colors. */
.ansibold {
  font-weight: bold;
}
/* use dark versions for foreground, to improve visibility */
.ansiblack {
  color: black;
}
.ansired {
  color: darkred;
}
.ansigreen {
  color: darkgreen;
}
.ansiyellow {
  color: #c4a000;
}
.ansiblue {
  color: darkblue;
}
.ansipurple {
  color: darkviolet;
}
.ansicyan {
  color: steelblue;
}
.ansigray {
  color: gray;
}
/* and light for background, for the same reason */
.ansibgblack {
  background-color: black;
}
.ansibgred {
  background-color: red;
}
.ansibggreen {
  background-color: green;
}
.ansibgyellow {
  background-color: yellow;
}
.ansibgblue {
  background-color: blue;
}
.ansibgpurple {
  background-color: magenta;
}
.ansibgcyan {
  background-color: cyan;
}
.ansibggray {
  background-color: gray;
}
div.cell {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  border-radius: 2px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  border-width: 1px;
  border-style: solid;
  border-color: transparent;
  width: 100%;
  padding: 5px;
  /* This acts as a spacer between cells, that is outside the border */
  margin: 0px;
  outline: none;
  border-left-width: 1px;
  padding-left: 5px;
  background: linear-gradient(to right, transparent -40px, transparent 1px, transparent 1px, transparent 100%);
}
div.cell.jupyter-soft-selected {
  border-left-color: #90CAF9;
  border-left-color: #E3F2FD;
  border-left-width: 1px;
  padding-left: 5px;
  border-right-color: #E3F2FD;
  border-right-width: 1px;
  background: #E3F2FD;
}
@media print {
  div.cell.jupyter-soft-selected {
    border-color: transparent;
  }
}
div.cell.selected {
  border-color: #ababab;
  border-left-width: 0px;
  padding-left: 6px;
  background: linear-gradient(to right, #42A5F5 -40px, #42A5F5 5px, transparent 5px, transparent 100%);
}
@media print {
  div.cell.selected {
    border-color: transparent;
  }
}
div.cell.selected.jupyter-soft-selected {
  border-left-width: 0;
  padding-left: 6px;
  background: linear-gradient(to right, #42A5F5 -40px, #42A5F5 7px, #E3F2FD 7px, #E3F2FD 100%);
}
.edit_mode div.cell.selected {
  border-color: #66BB6A;
  border-left-width: 0px;
  padding-left: 6px;
  background: linear-gradient(to right, #66BB6A -40px, #66BB6A 5px, transparent 5px, transparent 100%);
}
@media print {
  .edit_mode div.cell.selected {
    border-color: transparent;
  }
}
.prompt {
  /* This needs to be wide enough for 3 digit prompt numbers: In[100]: */
  min-width: 14ex;
  /* This padding is tuned to match the padding on the CodeMirror editor. */
  padding: 0.4em;
  margin: 0px;
  font-family: monospace;
  text-align: right;
  /* This has to match that of the the CodeMirror class line-height below */
  line-height: 1.21429em;
  /* Don't highlight prompt number selection */
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  /* Use default cursor */
  cursor: default;
}
@media (max-width: 540px) {
  .prompt {
    text-align: left;
  }
}
div.inner_cell {
  min-width: 0;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
/* input_area and input_prompt must match in top border and margin for alignment */
div.input_area {
  border: 1px solid #cfcfcf;
  border-radius: 2px;
  background: #f7f7f7;
  line-height: 1.21429em;
}
/* This is needed so that empty prompt areas can collapse to zero height when there
   is no content in the output_subarea and the prompt. The main purpose of this is
   to make sure that empty JavaScript output_subareas have no height. */
div.prompt:empty {
  padding-top: 0;
  padding-bottom: 0;
}
div.unrecognized_cell {
  padding: 5px 5px 5px 0px;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
div.unrecognized_cell .inner_cell {
  border-radius: 2px;
  padding: 5px;
  font-weight: bold;
  color: red;
  border: 1px solid #cfcfcf;
  background: #eaeaea;
}
div.unrecognized_cell .inner_cell a {
  color: inherit;
  text-decoration: none;
}
div.unrecognized_cell .inner_cell a:hover {
  color: inherit;
  text-decoration: none;
}
@media (max-width: 540px) {
  div.unrecognized_cell > div.prompt {
    display: none;
  }
}
div.code_cell {
  /* avoid page breaking on code cells when printing */
}
@media print {
  div.code_cell {
    page-break-inside: avoid;
  }
}
/* any special styling for code cells that are currently running goes here */
div.input {
  page-break-inside: avoid;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
@media (max-width: 540px) {
  div.input {
    /* Old browsers */
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-box-align: stretch;
    display: -moz-box;
    -moz-box-orient: vertical;
    -moz-box-align: stretch;
    display: box;
    box-orient: vertical;
    box-align: stretch;
    /* Modern browsers */
    display: flex;
    flex-direction: column;
    align-items: stretch;
  }
}
/* input_area and input_prompt must match in top border and margin for alignment */
div.input_prompt {
  color: #303F9F;
  border-top: 1px solid transparent;
}
div.input_area > div.highlight {
  margin: 0.4em;
  border: none;
  padding: 0px;
  background-color: transparent;
}
div.input_area > div.highlight > pre {
  margin: 0px;
  border: none;
  padding: 0px;
  background-color: transparent;
}
/* The following gets added to the <head> if it is detected that the user has a
 * monospace font with inconsistent normal/bold/italic height.  See
 * notebookmain.js.  Such fonts will have keywords vertically offset with
 * respect to the rest of the text.  The user should select a better font.
 * See: https://github.com/ipython/ipython/issues/1503
 *
 * .CodeMirror span {
 *      vertical-align: bottom;
 * }
 */
.CodeMirror {
  line-height: 1.21429em;
  /* Changed from 1em to our global default */
  font-size: 14px;
  height: auto;
  /* Changed to auto to autogrow */
  background: none;
  /* Changed from white to allow our bg to show through */
}
.CodeMirror-scroll {
  /*  The CodeMirror docs are a bit fuzzy on if overflow-y should be hidden or visible.*/
  /*  We have found that if it is visible, vertical scrollbars appear with font size changes.*/
  overflow-y: hidden;
  overflow-x: auto;
}
.CodeMirror-lines {
  /* In CM2, this used to be 0.4em, but in CM3 it went to 4px. We need the em value because */
  /* we have set a different line-height and want this to scale with that. */
  padding: 0.4em;
}
.CodeMirror-linenumber {
  padding: 0 8px 0 4px;
}
.CodeMirror-gutters {
  border-bottom-left-radius: 2px;
  border-top-left-radius: 2px;
}
.CodeMirror pre {
  /* In CM3 this went to 4px from 0 in CM2. We need the 0 value because of how we size */
  /* .CodeMirror-lines */
  padding: 0;
  border: 0;
  border-radius: 0;
}
/*

Original style from softwaremaniacs.org (c) Ivan Sagalaev <Maniac@SoftwareManiacs.Org>
Adapted from GitHub theme

*/
.highlight-base {
  color: #000;
}
.highlight-variable {
  color: #000;
}
.highlight-variable-2 {
  color: #1a1a1a;
}
.highlight-variable-3 {
  color: #333333;
}
.highlight-string {
  color: #BA2121;
}
.highlight-comment {
  color: #408080;
  font-style: italic;
}
.highlight-number {
  color: #080;
}
.highlight-atom {
  color: #88F;
}
.highlight-keyword {
  color: #008000;
  font-weight: bold;
}
.highlight-builtin {
  color: #008000;
}
.highlight-error {
  color: #f00;
}
.highlight-operator {
  color: #AA22FF;
  font-weight: bold;
}
.highlight-meta {
  color: #AA22FF;
}
/* previously not defined, copying from default codemirror */
.highlight-def {
  color: #00f;
}
.highlight-string-2 {
  color: #f50;
}
.highlight-qualifier {
  color: #555;
}
.highlight-bracket {
  color: #997;
}
.highlight-tag {
  color: #170;
}
.highlight-attribute {
  color: #00c;
}
.highlight-header {
  color: blue;
}
.highlight-quote {
  color: #090;
}
.highlight-link {
  color: #00c;
}
/* apply the same style to codemirror */
.cm-s-ipython span.cm-keyword {
  color: #008000;
  font-weight: bold;
}
.cm-s-ipython span.cm-atom {
  color: #88F;
}
.cm-s-ipython span.cm-number {
  color: #080;
}
.cm-s-ipython span.cm-def {
  color: #00f;
}
.cm-s-ipython span.cm-variable {
  color: #000;
}
.cm-s-ipython span.cm-operator {
  color: #AA22FF;
  font-weight: bold;
}
.cm-s-ipython span.cm-variable-2 {
  color: #1a1a1a;
}
.cm-s-ipython span.cm-variable-3 {
  color: #333333;
}
.cm-s-ipython span.cm-comment {
  color: #408080;
  font-style: italic;
}
.cm-s-ipython span.cm-string {
  color: #BA2121;
}
.cm-s-ipython span.cm-string-2 {
  color: #f50;
}
.cm-s-ipython span.cm-meta {
  color: #AA22FF;
}
.cm-s-ipython span.cm-qualifier {
  color: #555;
}
.cm-s-ipython span.cm-builtin {
  color: #008000;
}
.cm-s-ipython span.cm-bracket {
  color: #997;
}
.cm-s-ipython span.cm-tag {
  color: #170;
}
.cm-s-ipython span.cm-attribute {
  color: #00c;
}
.cm-s-ipython span.cm-header {
  color: blue;
}
.cm-s-ipython span.cm-quote {
  color: #090;
}
.cm-s-ipython span.cm-link {
  color: #00c;
}
.cm-s-ipython span.cm-error {
  color: #f00;
}
.cm-s-ipython span.cm-tab {
  background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAMCAYAAAAkuj5RAAAAAXNSR0IArs4c6QAAAGFJREFUSMft1LsRQFAQheHPowAKoACx3IgEKtaEHujDjORSgWTH/ZOdnZOcM/sgk/kFFWY0qV8foQwS4MKBCS3qR6ixBJvElOobYAtivseIE120FaowJPN75GMu8j/LfMwNjh4HUpwg4LUAAAAASUVORK5CYII=);
  background-position: right;
  background-repeat: no-repeat;
}
div.output_wrapper {
  /* this position must be relative to enable descendents to be absolute within it */
  position: relative;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  z-index: 1;
}
/* class for the output area when it should be height-limited */
div.output_scroll {
  /* ideally, this would be max-height, but FF barfs all over that */
  height: 24em;
  /* FF needs this *and the wrapper* to specify full width, or it will shrinkwrap */
  width: 100%;
  overflow: auto;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 2px 8px rgba(0, 0, 0, 0.8);
  box-shadow: inset 0 2px 8px rgba(0, 0, 0, 0.8);
  display: block;
}
/* output div while it is collapsed */
div.output_collapsed {
  margin: 0px;
  padding: 0px;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
}
div.out_prompt_overlay {
  height: 100%;
  padding: 0px 0.4em;
  position: absolute;
  border-radius: 2px;
}
div.out_prompt_overlay:hover {
  /* use inner shadow to get border that is computed the same on WebKit/FF */
  -webkit-box-shadow: inset 0 0 1px #000;
  box-shadow: inset 0 0 1px #000;
  background: rgba(240, 240, 240, 0.5);
}
div.output_prompt {
  color: #D84315;
}
/* This class is the outer container of all output sections. */
div.output_area {
  padding: 0px;
  page-break-inside: avoid;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
div.output_area .MathJax_Display {
  text-align: left !important;
}
div.output_area .rendered_html table {
  margin-left: 0;
  margin-right: 0;
}
div.output_area .rendered_html img {
  margin-left: 0;
  margin-right: 0;
}
div.output_area img,
div.output_area svg {
  max-width: 100%;
  height: auto;
}
div.output_area img.unconfined,
div.output_area svg.unconfined {
  max-width: none;
}
/* This is needed to protect the pre formating from global settings such
   as that of bootstrap */
.output {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
}
@media (max-width: 540px) {
  div.output_area {
    /* Old browsers */
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-box-align: stretch;
    display: -moz-box;
    -moz-box-orient: vertical;
    -moz-box-align: stretch;
    display: box;
    box-orient: vertical;
    box-align: stretch;
    /* Modern browsers */
    display: flex;
    flex-direction: column;
    align-items: stretch;
  }
}
div.output_area pre {
  margin: 0;
  padding: 0;
  border: 0;
  vertical-align: baseline;
  color: black;
  background-color: transparent;
  border-radius: 0;
}
/* This class is for the output subarea inside the output_area and after
   the prompt div. */
div.output_subarea {
  overflow-x: auto;
  padding: 0.4em;
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
  max-width: calc(100% - 14ex);
}
div.output_scroll div.output_subarea {
  overflow-x: visible;
}
/* The rest of the output_* classes are for special styling of the different
   output types */
/* all text output has this class: */
div.output_text {
  text-align: left;
  color: #000;
  /* This has to match that of the the CodeMirror class line-height below */
  line-height: 1.21429em;
}
/* stdout/stderr are 'text' as well as 'stream', but execute_result/error are *not* streams */
div.output_stderr {
  background: #fdd;
  /* very light red background for stderr */
}
div.output_latex {
  text-align: left;
}
/* Empty output_javascript divs should have no height */
div.output_javascript:empty {
  padding: 0;
}
.js-error {
  color: darkred;
}
/* raw_input styles */
div.raw_input_container {
  line-height: 1.21429em;
  padding-top: 5px;
}
pre.raw_input_prompt {
  /* nothing needed here. */
}
input.raw_input {
  font-family: monospace;
  font-size: inherit;
  color: inherit;
  width: auto;
  /* make sure input baseline aligns with prompt */
  vertical-align: baseline;
  /* padding + margin = 0.5em between prompt and cursor */
  padding: 0em 0.25em;
  margin: 0em 0.25em;
}
input.raw_input:focus {
  box-shadow: none;
}
p.p-space {
  margin-bottom: 10px;
}
div.output_unrecognized {
  padding: 5px;
  font-weight: bold;
  color: red;
}
div.output_unrecognized a {
  color: inherit;
  text-decoration: none;
}
div.output_unrecognized a:hover {
  color: inherit;
  text-decoration: none;
}
.rendered_html {
  color: #000;
  /* any extras will just be numbers: */
}
.rendered_html em {
  font-style: italic;
}
.rendered_html strong {
  font-weight: bold;
}
.rendered_html u {
  text-decoration: underline;
}
.rendered_html :link {
  text-decoration: underline;
}
.rendered_html :visited {
  text-decoration: underline;
}
.rendered_html h1 {
  font-size: 185.7%;
  margin: 1.08em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h2 {
  font-size: 157.1%;
  margin: 1.27em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h3 {
  font-size: 128.6%;
  margin: 1.55em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h4 {
  font-size: 100%;
  margin: 2em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h5 {
  font-size: 100%;
  margin: 2em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
  font-style: italic;
}
.rendered_html h6 {
  font-size: 100%;
  margin: 2em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
  font-style: italic;
}
.rendered_html h1:first-child {
  margin-top: 0.538em;
}
.rendered_html h2:first-child {
  margin-top: 0.636em;
}
.rendered_html h3:first-child {
  margin-top: 0.777em;
}
.rendered_html h4:first-child {
  margin-top: 1em;
}
.rendered_html h5:first-child {
  margin-top: 1em;
}
.rendered_html h6:first-child {
  margin-top: 1em;
}
.rendered_html ul {
  list-style: disc;
  margin: 0em 2em;
  padding-left: 0px;
}
.rendered_html ul ul {
  list-style: square;
  margin: 0em 2em;
}
.rendered_html ul ul ul {
  list-style: circle;
  margin: 0em 2em;
}
.rendered_html ol {
  list-style: decimal;
  margin: 0em 2em;
  padding-left: 0px;
}
.rendered_html ol ol {
  list-style: upper-alpha;
  margin: 0em 2em;
}
.rendered_html ol ol ol {
  list-style: lower-alpha;
  margin: 0em 2em;
}
.rendered_html ol ol ol ol {
  list-style: lower-roman;
  margin: 0em 2em;
}
.rendered_html ol ol ol ol ol {
  list-style: decimal;
  margin: 0em 2em;
}
.rendered_html * + ul {
  margin-top: 1em;
}
.rendered_html * + ol {
  margin-top: 1em;
}
.rendered_html hr {
  color: black;
  background-color: black;
}
.rendered_html pre {
  margin: 1em 2em;
}
.rendered_html pre,
.rendered_html code {
  border: 0;
  background-color: #fff;
  color: #000;
  font-size: 100%;
  padding: 0px;
}
.rendered_html blockquote {
  margin: 1em 2em;
}
.rendered_html table {
  margin-left: auto;
  margin-right: auto;
  border: 1px solid black;
  border-collapse: collapse;
}
.rendered_html tr,
.rendered_html th,
.rendered_html td {
  border: 1px solid black;
  border-collapse: collapse;
  margin: 1em 2em;
}
.rendered_html td,
.rendered_html th {
  text-align: left;
  vertical-align: middle;
  padding: 4px;
}
.rendered_html th {
  font-weight: bold;
}
.rendered_html * + table {
  margin-top: 1em;
}
.rendered_html p {
  text-align: left;
}
.rendered_html * + p {
  margin-top: 1em;
}
.rendered_html img {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
.rendered_html * + img {
  margin-top: 1em;
}
.rendered_html img,
.rendered_html svg {
  max-width: 100%;
  height: auto;
}
.rendered_html img.unconfined,
.rendered_html svg.unconfined {
  max-width: none;
}
div.text_cell {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
@media (max-width: 540px) {
  div.text_cell > div.prompt {
    display: none;
  }
}
div.text_cell_render {
  /*font-family: "Helvetica Neue", Arial, Helvetica, Geneva, sans-serif;*/
  outline: none;
  resize: none;
  width: inherit;
  border-style: none;
  padding: 0.5em 0.5em 0.5em 0.4em;
  color: #000;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
a.anchor-link:link {
  text-decoration: none;
  padding: 0px 20px;
  visibility: hidden;
}
h1:hover .anchor-link,
h2:hover .anchor-link,
h3:hover .anchor-link,
h4:hover .anchor-link,
h5:hover .anchor-link,
h6:hover .anchor-link {
  visibility: visible;
}
.text_cell.rendered .input_area {
  display: none;
}
.text_cell.rendered .rendered_html {
  overflow-x: auto;
  overflow-y: hidden;
}
.text_cell.unrendered .text_cell_render {
  display: none;
}
.cm-header-1,
.cm-header-2,
.cm-header-3,
.cm-header-4,
.cm-header-5,
.cm-header-6 {
  font-weight: bold;
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
}
.cm-header-1 {
  font-size: 185.7%;
}
.cm-header-2 {
  font-size: 157.1%;
}
.cm-header-3 {
  font-size: 128.6%;
}
.cm-header-4 {
  font-size: 110%;
}
.cm-header-5 {
  font-size: 100%;
  font-style: italic;
}
.cm-header-6 {
  font-size: 100%;
  font-style: italic;
}
/*!
*
* IPython notebook webapp
*
*/
@media (max-width: 767px) {
  .notebook_app {
    padding-left: 0px;
    padding-right: 0px;
  }
}
#ipython-main-app {
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  height: 100%;
}
div#notebook_panel {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  height: 100%;
}
div#notebook {
  font-size: 14px;
  line-height: 20px;
  overflow-y: hidden;
  overflow-x: auto;
  width: 100%;
  /* This spaces the page away from the edge of the notebook area */
  padding-top: 20px;
  margin: 0px;
  outline: none;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  min-height: 100%;
}
@media not print {
  #notebook-container {
    padding: 15px;
    background-color: #fff;
    min-height: 0;
    -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
    box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  }
}
@media print {
  #notebook-container {
    width: 100%;
  }
}
div.ui-widget-content {
  border: 1px solid #ababab;
  outline: none;
}
pre.dialog {
  background-color: #f7f7f7;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0.4em;
  padding-left: 2em;
}
p.dialog {
  padding: 0.2em;
}
/* Word-wrap output correctly.  This is the CSS3 spelling, though Firefox seems
   to not honor it correctly.  Webkit browsers (Chrome, rekonq, Safari) do.
 */
pre,
code,
kbd,
samp {
  white-space: pre-wrap;
}
#fonttest {
  font-family: monospace;
}
p {
  margin-bottom: 0;
}
.end_space {
  min-height: 100px;
  transition: height .2s ease;
}
.notebook_app > #header {
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
}
@media not print {
  .notebook_app {
    background-color: #EEE;
  }
}
kbd {
  border-style: solid;
  border-width: 1px;
  box-shadow: none;
  margin: 2px;
  padding-left: 2px;
  padding-right: 2px;
  padding-top: 1px;
  padding-bottom: 1px;
}
/* CSS for the cell toolbar */
.celltoolbar {
  border: thin solid #CFCFCF;
  border-bottom: none;
  background: #EEE;
  border-radius: 2px 2px 0px 0px;
  width: 100%;
  height: 29px;
  padding-right: 4px;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
  /* Old browsers */
  -webkit-box-pack: end;
  -moz-box-pack: end;
  box-pack: end;
  /* Modern browsers */
  justify-content: flex-end;
  display: -webkit-flex;
}
@media print {
  .celltoolbar {
    display: none;
  }
}
.ctb_hideshow {
  display: none;
  vertical-align: bottom;
}
/* ctb_show is added to the ctb_hideshow div to show the cell toolbar.
   Cell toolbars are only shown when the ctb_global_show class is also set.
*/
.ctb_global_show .ctb_show.ctb_hideshow {
  display: block;
}
.ctb_global_show .ctb_show + .input_area,
.ctb_global_show .ctb_show + div.text_cell_input,
.ctb_global_show .ctb_show ~ div.text_cell_render {
  border-top-right-radius: 0px;
  border-top-left-radius: 0px;
}
.ctb_global_show .ctb_show ~ div.text_cell_render {
  border: 1px solid #cfcfcf;
}
.celltoolbar {
  font-size: 87%;
  padding-top: 3px;
}
.celltoolbar select {
  display: block;
  width: 100%;
  height: 32px;
  padding: 6px 12px;
  font-size: 13px;
  line-height: 1.42857143;
  color: #555555;
  background-color: #fff;
  background-image: none;
  border: 1px solid #ccc;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  -webkit-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  -o-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
  width: inherit;
  font-size: inherit;
  height: 22px;
  padding: 0px;
  display: inline-block;
}
.celltoolbar select:focus {
  border-color: #66afe9;
  outline: 0;
  -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
  box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
}
.celltoolbar select::-moz-placeholder {
  color: #999;
  opacity: 1;
}
.celltoolbar select:-ms-input-placeholder {
  color: #999;
}
.celltoolbar select::-webkit-input-placeholder {
  color: #999;
}
.celltoolbar select::-ms-expand {
  border: 0;
  background-color: transparent;
}
.celltoolbar select[disabled],
.celltoolbar select[readonly],
fieldset[disabled] .celltoolbar select {
  background-color: #eeeeee;
  opacity: 1;
}
.celltoolbar select[disabled],
fieldset[disabled] .celltoolbar select {
  cursor: not-allowed;
}
textarea.celltoolbar select {
  height: auto;
}
select.celltoolbar select {
  height: 30px;
  line-height: 30px;
}
textarea.celltoolbar select,
select[multiple].celltoolbar select {
  height: auto;
}
.celltoolbar label {
  margin-left: 5px;
  margin-right: 5px;
}
.completions {
  position: absolute;
  z-index: 110;
  overflow: hidden;
  border: 1px solid #ababab;
  border-radius: 2px;
  -webkit-box-shadow: 0px 6px 10px -1px #adadad;
  box-shadow: 0px 6px 10px -1px #adadad;
  line-height: 1;
}
.completions select {
  background: white;
  outline: none;
  border: none;
  padding: 0px;
  margin: 0px;
  overflow: auto;
  font-family: monospace;
  font-size: 110%;
  color: #000;
  width: auto;
}
.completions select option.context {
  color: #286090;
}
#kernel_logo_widget {
  float: right !important;
  float: right;
}
#kernel_logo_widget .current_kernel_logo {
  display: none;
  margin-top: -1px;
  margin-bottom: -1px;
  width: 32px;
  height: 32px;
}
#menubar {
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  margin-top: 1px;
}
#menubar .navbar {
  border-top: 1px;
  border-radius: 0px 0px 2px 2px;
  margin-bottom: 0px;
}
#menubar .navbar-toggle {
  float: left;
  padding-top: 7px;
  padding-bottom: 7px;
  border: none;
}
#menubar .navbar-collapse {
  clear: left;
}
.nav-wrapper {
  border-bottom: 1px solid #e7e7e7;
}
i.menu-icon {
  padding-top: 4px;
}
ul#help_menu li a {
  overflow: hidden;
  padding-right: 2.2em;
}
ul#help_menu li a i {
  margin-right: -1.2em;
}
.dropdown-submenu {
  position: relative;
}
.dropdown-submenu > .dropdown-menu {
  top: 0;
  left: 100%;
  margin-top: -6px;
  margin-left: -1px;
}
.dropdown-submenu:hover > .dropdown-menu {
  display: block;
}
.dropdown-submenu > a:after {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: block;
  content: "\f0da";
  float: right;
  color: #333333;
  margin-top: 2px;
  margin-right: -10px;
}
.dropdown-submenu > a:after.pull-left {
  margin-right: .3em;
}
.dropdown-submenu > a:after.pull-right {
  margin-left: .3em;
}
.dropdown-submenu:hover > a:after {
  color: #262626;
}
.dropdown-submenu.pull-left {
  float: none;
}
.dropdown-submenu.pull-left > .dropdown-menu {
  left: -100%;
  margin-left: 10px;
}
#notification_area {
  float: right !important;
  float: right;
  z-index: 10;
}
.indicator_area {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
}
#kernel_indicator {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
  border-left: 1px solid;
}
#kernel_indicator .kernel_indicator_name {
  padding-left: 5px;
  padding-right: 5px;
}
#modal_indicator {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
}
#readonly-indicator {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
  margin-top: 2px;
  margin-bottom: 0px;
  margin-left: 0px;
  margin-right: 0px;
  display: none;
}
.modal_indicator:before {
  width: 1.28571429em;
  text-align: center;
}
.edit_mode .modal_indicator:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f040";
}
.edit_mode .modal_indicator:before.pull-left {
  margin-right: .3em;
}
.edit_mode .modal_indicator:before.pull-right {
  margin-left: .3em;
}
.command_mode .modal_indicator:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: ' ';
}
.command_mode .modal_indicator:before.pull-left {
  margin-right: .3em;
}
.command_mode .modal_indicator:before.pull-right {
  margin-left: .3em;
}
.kernel_idle_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f10c";
}
.kernel_idle_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_idle_icon:before.pull-right {
  margin-left: .3em;
}
.kernel_busy_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f111";
}
.kernel_busy_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_busy_icon:before.pull-right {
  margin-left: .3em;
}
.kernel_dead_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f1e2";
}
.kernel_dead_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_dead_icon:before.pull-right {
  margin-left: .3em;
}
.kernel_disconnected_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f127";
}
.kernel_disconnected_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_disconnected_icon:before.pull-right {
  margin-left: .3em;
}
.notification_widget {
  color: #777;
  z-index: 10;
  background: rgba(240, 240, 240, 0.5);
  margin-right: 4px;
  color: #333;
  background-color: #fff;
  border-color: #ccc;
}
.notification_widget:focus,
.notification_widget.focus {
  color: #333;
  background-color: #e6e6e6;
  border-color: #8c8c8c;
}
.notification_widget:hover {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.notification_widget:active,
.notification_widget.active,
.open > .dropdown-toggle.notification_widget {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.notification_widget:active:hover,
.notification_widget.active:hover,
.open > .dropdown-toggle.notification_widget:hover,
.notification_widget:active:focus,
.notification_widget.active:focus,
.open > .dropdown-toggle.notification_widget:focus,
.notification_widget:active.focus,
.notification_widget.active.focus,
.open > .dropdown-toggle.notification_widget.focus {
  color: #333;
  background-color: #d4d4d4;
  border-color: #8c8c8c;
}
.notification_widget:active,
.notification_widget.active,
.open > .dropdown-toggle.notification_widget {
  background-image: none;
}
.notification_widget.disabled:hover,
.notification_widget[disabled]:hover,
fieldset[disabled] .notification_widget:hover,
.notification_widget.disabled:focus,
.notification_widget[disabled]:focus,
fieldset[disabled] .notification_widget:focus,
.notification_widget.disabled.focus,
.notification_widget[disabled].focus,
fieldset[disabled] .notification_widget.focus {
  background-color: #fff;
  border-color: #ccc;
}
.notification_widget .badge {
  color: #fff;
  background-color: #333;
}
.notification_widget.warning {
  color: #fff;
  background-color: #f0ad4e;
  border-color: #eea236;
}
.notification_widget.warning:focus,
.notification_widget.warning.focus {
  color: #fff;
  background-color: #ec971f;
  border-color: #985f0d;
}
.notification_widget.warning:hover {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.notification_widget.warning:active,
.notification_widget.warning.active,
.open > .dropdown-toggle.notification_widget.warning {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.notification_widget.warning:active:hover,
.notification_widget.warning.active:hover,
.open > .dropdown-toggle.notification_widget.warning:hover,
.notification_widget.warning:active:focus,
.notification_widget.warning.active:focus,
.open > .dropdown-toggle.notification_widget.warning:focus,
.notification_widget.warning:active.focus,
.notification_widget.warning.active.focus,
.open > .dropdown-toggle.notification_widget.warning.focus {
  color: #fff;
  background-color: #d58512;
  border-color: #985f0d;
}
.notification_widget.warning:active,
.notification_widget.warning.active,
.open > .dropdown-toggle.notification_widget.warning {
  background-image: none;
}
.notification_widget.warning.disabled:hover,
.notification_widget.warning[disabled]:hover,
fieldset[disabled] .notification_widget.warning:hover,
.notification_widget.warning.disabled:focus,
.notification_widget.warning[disabled]:focus,
fieldset[disabled] .notification_widget.warning:focus,
.notification_widget.warning.disabled.focus,
.notification_widget.warning[disabled].focus,
fieldset[disabled] .notification_widget.warning.focus {
  background-color: #f0ad4e;
  border-color: #eea236;
}
.notification_widget.warning .badge {
  color: #f0ad4e;
  background-color: #fff;
}
.notification_widget.success {
  color: #fff;
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.notification_widget.success:focus,
.notification_widget.success.focus {
  color: #fff;
  background-color: #449d44;
  border-color: #255625;
}
.notification_widget.success:hover {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.notification_widget.success:active,
.notification_widget.success.active,
.open > .dropdown-toggle.notification_widget.success {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.notification_widget.success:active:hover,
.notification_widget.success.active:hover,
.open > .dropdown-toggle.notification_widget.success:hover,
.notification_widget.success:active:focus,
.notification_widget.success.active:focus,
.open > .dropdown-toggle.notification_widget.success:focus,
.notification_widget.success:active.focus,
.notification_widget.success.active.focus,
.open > .dropdown-toggle.notification_widget.success.focus {
  color: #fff;
  background-color: #398439;
  border-color: #255625;
}
.notification_widget.success:active,
.notification_widget.success.active,
.open > .dropdown-toggle.notification_widget.success {
  background-image: none;
}
.notification_widget.success.disabled:hover,
.notification_widget.success[disabled]:hover,
fieldset[disabled] .notification_widget.success:hover,
.notification_widget.success.disabled:focus,
.notification_widget.success[disabled]:focus,
fieldset[disabled] .notification_widget.success:focus,
.notification_widget.success.disabled.focus,
.notification_widget.success[disabled].focus,
fieldset[disabled] .notification_widget.success.focus {
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.notification_widget.success .badge {
  color: #5cb85c;
  background-color: #fff;
}
.notification_widget.info {
  color: #fff;
  background-color: #5bc0de;
  border-color: #46b8da;
}
.notification_widget.info:focus,
.notification_widget.info.focus {
  color: #fff;
  background-color: #31b0d5;
  border-color: #1b6d85;
}
.notification_widget.info:hover {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.notification_widget.info:active,
.notification_widget.info.active,
.open > .dropdown-toggle.notification_widget.info {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.notification_widget.info:active:hover,
.notification_widget.info.active:hover,
.open > .dropdown-toggle.notification_widget.info:hover,
.notification_widget.info:active:focus,
.notification_widget.info.active:focus,
.open > .dropdown-toggle.notification_widget.info:focus,
.notification_widget.info:active.focus,
.notification_widget.info.active.focus,
.open > .dropdown-toggle.notification_widget.info.focus {
  color: #fff;
  background-color: #269abc;
  border-color: #1b6d85;
}
.notification_widget.info:active,
.notification_widget.info.active,
.open > .dropdown-toggle.notification_widget.info {
  background-image: none;
}
.notification_widget.info.disabled:hover,
.notification_widget.info[disabled]:hover,
fieldset[disabled] .notification_widget.info:hover,
.notification_widget.info.disabled:focus,
.notification_widget.info[disabled]:focus,
fieldset[disabled] .notification_widget.info:focus,
.notification_widget.info.disabled.focus,
.notification_widget.info[disabled].focus,
fieldset[disabled] .notification_widget.info.focus {
  background-color: #5bc0de;
  border-color: #46b8da;
}
.notification_widget.info .badge {
  color: #5bc0de;
  background-color: #fff;
}
.notification_widget.danger {
  color: #fff;
  background-color: #d9534f;
  border-color: #d43f3a;
}
.notification_widget.danger:focus,
.notification_widget.danger.focus {
  color: #fff;
  background-color: #c9302c;
  border-color: #761c19;
}
.notification_widget.danger:hover {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.notification_widget.danger:active,
.notification_widget.danger.active,
.open > .dropdown-toggle.notification_widget.danger {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.notification_widget.danger:active:hover,
.notification_widget.danger.active:hover,
.open > .dropdown-toggle.notification_widget.danger:hover,
.notification_widget.danger:active:focus,
.notification_widget.danger.active:focus,
.open > .dropdown-toggle.notification_widget.danger:focus,
.notification_widget.danger:active.focus,
.notification_widget.danger.active.focus,
.open > .dropdown-toggle.notification_widget.danger.focus {
  color: #fff;
  background-color: #ac2925;
  border-color: #761c19;
}
.notification_widget.danger:active,
.notification_widget.danger.active,
.open > .dropdown-toggle.notification_widget.danger {
  background-image: none;
}
.notification_widget.danger.disabled:hover,
.notification_widget.danger[disabled]:hover,
fieldset[disabled] .notification_widget.danger:hover,
.notification_widget.danger.disabled:focus,
.notification_widget.danger[disabled]:focus,
fieldset[disabled] .notification_widget.danger:focus,
.notification_widget.danger.disabled.focus,
.notification_widget.danger[disabled].focus,
fieldset[disabled] .notification_widget.danger.focus {
  background-color: #d9534f;
  border-color: #d43f3a;
}
.notification_widget.danger .badge {
  color: #d9534f;
  background-color: #fff;
}
div#pager {
  background-color: #fff;
  font-size: 14px;
  line-height: 20px;
  overflow: hidden;
  display: none;
  position: fixed;
  bottom: 0px;
  width: 100%;
  max-height: 50%;
  padding-top: 8px;
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  /* Display over codemirror */
  z-index: 100;
  /* Hack which prevents jquery ui resizable from changing top. */
  top: auto !important;
}
div#pager pre {
  line-height: 1.21429em;
  color: #000;
  background-color: #f7f7f7;
  padding: 0.4em;
}
div#pager #pager-button-area {
  position: absolute;
  top: 8px;
  right: 20px;
}
div#pager #pager-contents {
  position: relative;
  overflow: auto;
  width: 100%;
  height: 100%;
}
div#pager #pager-contents #pager-container {
  position: relative;
  padding: 15px 0px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
div#pager .ui-resizable-handle {
  top: 0px;
  height: 8px;
  background: #f7f7f7;
  border-top: 1px solid #cfcfcf;
  border-bottom: 1px solid #cfcfcf;
  /* This injects handle bars (a short, wide = symbol) for 
        the resize handle. */
}
div#pager .ui-resizable-handle::after {
  content: '';
  top: 2px;
  left: 50%;
  height: 3px;
  width: 30px;
  margin-left: -15px;
  position: absolute;
  border-top: 1px solid #cfcfcf;
}
.quickhelp {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
  line-height: 1.8em;
}
.shortcut_key {
  display: inline-block;
  width: 21ex;
  text-align: right;
  font-family: monospace;
}
.shortcut_descr {
  display: inline-block;
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
span.save_widget {
  margin-top: 6px;
}
span.save_widget span.filename {
  height: 1em;
  line-height: 1em;
  padding: 3px;
  margin-left: 16px;
  border: none;
  font-size: 146.5%;
  border-radius: 2px;
}
span.save_widget span.filename:hover {
  background-color: #e6e6e6;
}
span.checkpoint_status,
span.autosave_status {
  font-size: small;
}
@media (max-width: 767px) {
  span.save_widget {
    font-size: small;
  }
  span.checkpoint_status,
  span.autosave_status {
    display: none;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  span.checkpoint_status {
    display: none;
  }
  span.autosave_status {
    font-size: x-small;
  }
}
.toolbar {
  padding: 0px;
  margin-left: -5px;
  margin-top: 2px;
  margin-bottom: 5px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
.toolbar select,
.toolbar label {
  width: auto;
  vertical-align: middle;
  margin-right: 2px;
  margin-bottom: 0px;
  display: inline;
  font-size: 92%;
  margin-left: 0.3em;
  margin-right: 0.3em;
  padding: 0px;
  padding-top: 3px;
}
.toolbar .btn {
  padding: 2px 8px;
}
.toolbar .btn-group {
  margin-top: 0px;
  margin-left: 5px;
}
#maintoolbar {
  margin-bottom: -3px;
  margin-top: -8px;
  border: 0px;
  min-height: 27px;
  margin-left: 0px;
  padding-top: 11px;
  padding-bottom: 3px;
}
#maintoolbar .navbar-text {
  float: none;
  vertical-align: middle;
  text-align: right;
  margin-left: 5px;
  margin-right: 0px;
  margin-top: 0px;
}
.select-xs {
  height: 24px;
}
.pulse,
.dropdown-menu > li > a.pulse,
li.pulse > a.dropdown-toggle,
li.pulse.open > a.dropdown-toggle {
  background-color: #F37626;
  color: white;
}
/**
 * Primary styles
 *
 * Author: Jupyter Development Team
 */
/** WARNING IF YOU ARE EDITTING THIS FILE, if this is a .css file, It has a lot
 * of chance of beeing generated from the ../less/[samename].less file, you can
 * try to get back the less file by reverting somme commit in history
 **/
/*
 * We'll try to get something pretty, so we
 * have some strange css to have the scroll bar on
 * the left with fix button on the top right of the tooltip
 */
@-moz-keyframes fadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
@-webkit-keyframes fadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
@-moz-keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
@-webkit-keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
/*properties of tooltip after "expand"*/
.bigtooltip {
  overflow: auto;
  height: 200px;
  -webkit-transition-property: height;
  -webkit-transition-duration: 500ms;
  -moz-transition-property: height;
  -moz-transition-duration: 500ms;
  transition-property: height;
  transition-duration: 500ms;
}
/*properties of tooltip before "expand"*/
.smalltooltip {
  -webkit-transition-property: height;
  -webkit-transition-duration: 500ms;
  -moz-transition-property: height;
  -moz-transition-duration: 500ms;
  transition-property: height;
  transition-duration: 500ms;
  text-overflow: ellipsis;
  overflow: hidden;
  height: 80px;
}
.tooltipbuttons {
  position: absolute;
  padding-right: 15px;
  top: 0px;
  right: 0px;
}
.tooltiptext {
  /*avoid the button to overlap on some docstring*/
  padding-right: 30px;
}
.ipython_tooltip {
  max-width: 700px;
  /*fade-in animation when inserted*/
  -webkit-animation: fadeOut 400ms;
  -moz-animation: fadeOut 400ms;
  animation: fadeOut 400ms;
  -webkit-animation: fadeIn 400ms;
  -moz-animation: fadeIn 400ms;
  animation: fadeIn 400ms;
  vertical-align: middle;
  background-color: #f7f7f7;
  overflow: visible;
  border: #ababab 1px solid;
  outline: none;
  padding: 3px;
  margin: 0px;
  padding-left: 7px;
  font-family: monospace;
  min-height: 50px;
  -moz-box-shadow: 0px 6px 10px -1px #adadad;
  -webkit-box-shadow: 0px 6px 10px -1px #adadad;
  box-shadow: 0px 6px 10px -1px #adadad;
  border-radius: 2px;
  position: absolute;
  z-index: 1000;
}
.ipython_tooltip a {
  float: right;
}
.ipython_tooltip .tooltiptext pre {
  border: 0;
  border-radius: 0;
  font-size: 100%;
  background-color: #f7f7f7;
}
.pretooltiparrow {
  left: 0px;
  margin: 0px;
  top: -16px;
  width: 40px;
  height: 16px;
  overflow: hidden;
  position: absolute;
}
.pretooltiparrow:before {
  background-color: #f7f7f7;
  border: 1px #ababab solid;
  z-index: 11;
  content: "";
  position: absolute;
  left: 15px;
  top: 10px;
  width: 25px;
  height: 25px;
  -webkit-transform: rotate(45deg);
  -moz-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  -o-transform: rotate(45deg);
}
ul.typeahead-list i {
  margin-left: -10px;
  width: 18px;
}
ul.typeahead-list {
  max-height: 80vh;
  overflow: auto;
}
ul.typeahead-list > li > a {
  /** Firefox bug **/
  /* see https://github.com/jupyter/notebook/issues/559 */
  white-space: normal;
}
.cmd-palette .modal-body {
  padding: 7px;
}
.cmd-palette form {
  background: white;
}
.cmd-palette input {
  outline: none;
}
.no-shortcut {
  display: none;
}
.command-shortcut:before {
  content: "(command)";
  padding-right: 3px;
  color: #777777;
}
.edit-shortcut:before {
  content: "(edit)";
  padding-right: 3px;
  color: #777777;
}
#find-and-replace #replace-preview .match,
#find-and-replace #replace-preview .insert {
  background-color: #BBDEFB;
  border-color: #90CAF9;
  border-style: solid;
  border-width: 1px;
  border-radius: 0px;
}
#find-and-replace #replace-preview .replace .match {
  background-color: #FFCDD2;
  border-color: #EF9A9A;
  border-radius: 0px;
}
#find-and-replace #replace-preview .replace .insert {
  background-color: #C8E6C9;
  border-color: #A5D6A7;
  border-radius: 0px;
}
#find-and-replace #replace-preview {
  max-height: 60vh;
  overflow: auto;
}
#find-and-replace #replace-preview pre {
  padding: 5px 10px;
}
.terminal-app {
  background: #EEE;
}
.terminal-app #header {
  background: #fff;
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
}
.terminal-app .terminal {
  width: 100%;
  float: left;
  font-family: monospace;
  color: white;
  background: black;
  padding: 0.4em;
  border-radius: 2px;
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.4);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.4);
}
.terminal-app .terminal,
.terminal-app .terminal dummy-screen {
  line-height: 1em;
  font-size: 14px;
}
.terminal-app .terminal .xterm-rows {
  padding: 10px;
}
.terminal-app .terminal-cursor {
  color: black;
  background: white;
}
.terminal-app #terminado-container {
  margin-top: 20px;
}
/*# sourceMappingURL=style.min.css.map */
    </style>
<style type="text/css">
    .highlight .hll { background-color: #ffffcc }
.highlight  { background: #f8f8f8; }
.highlight .c { color: #408080; font-style: italic } /* Comment */
.highlight .err { border: 1px solid #FF0000 } /* Error */
.highlight .k { color: #008000; font-weight: bold } /* Keyword */
.highlight .o { color: #666666 } /* Operator */
.highlight .ch { color: #408080; font-style: italic } /* Comment.Hashbang */
.highlight .cm { color: #408080; font-style: italic } /* Comment.Multiline */
.highlight .cp { color: #BC7A00 } /* Comment.Preproc */
.highlight .cpf { color: #408080; font-style: italic } /* Comment.PreprocFile */
.highlight .c1 { color: #408080; font-style: italic } /* Comment.Single */
.highlight .cs { color: #408080; font-style: italic } /* Comment.Special */
.highlight .gd { color: #A00000 } /* Generic.Deleted */
.highlight .ge { font-style: italic } /* Generic.Emph */
.highlight .gr { color: #FF0000 } /* Generic.Error */
.highlight .gh { color: #000080; font-weight: bold } /* Generic.Heading */
.highlight .gi { color: #00A000 } /* Generic.Inserted */
.highlight .go { color: #888888 } /* Generic.Output */
.highlight .gp { color: #000080; font-weight: bold } /* Generic.Prompt */
.highlight .gs { font-weight: bold } /* Generic.Strong */
.highlight .gu { color: #800080; font-weight: bold } /* Generic.Subheading */
.highlight .gt { color: #0044DD } /* Generic.Traceback */
.highlight .kc { color: #008000; font-weight: bold } /* Keyword.Constant */
.highlight .kd { color: #008000; font-weight: bold } /* Keyword.Declaration */
.highlight .kn { color: #008000; font-weight: bold } /* Keyword.Namespace */
.highlight .kp { color: #008000 } /* Keyword.Pseudo */
.highlight .kr { color: #008000; font-weight: bold } /* Keyword.Reserved */
.highlight .kt { color: #B00040 } /* Keyword.Type */
.highlight .m { color: #666666 } /* Literal.Number */
.highlight .s { color: #BA2121 } /* Literal.String */
.highlight .na { color: #7D9029 } /* Name.Attribute */
.highlight .nb { color: #008000 } /* Name.Builtin */
.highlight .nc { color: #0000FF; font-weight: bold } /* Name.Class */
.highlight .no { color: #880000 } /* Name.Constant */
.highlight .nd { color: #AA22FF } /* Name.Decorator */
.highlight .ni { color: #999999; font-weight: bold } /* Name.Entity */
.highlight .ne { color: #D2413A; font-weight: bold } /* Name.Exception */
.highlight .nf { color: #0000FF } /* Name.Function */
.highlight .nl { color: #A0A000 } /* Name.Label */
.highlight .nn { color: #0000FF; font-weight: bold } /* Name.Namespace */
.highlight .nt { color: #008000; font-weight: bold } /* Name.Tag */
.highlight .nv { color: #19177C } /* Name.Variable */
.highlight .ow { color: #AA22FF; font-weight: bold } /* Operator.Word */
.highlight .w { color: #bbbbbb } /* Text.Whitespace */
.highlight .mb { color: #666666 } /* Literal.Number.Bin */
.highlight .mf { color: #666666 } /* Literal.Number.Float */
.highlight .mh { color: #666666 } /* Literal.Number.Hex */
.highlight .mi { color: #666666 } /* Literal.Number.Integer */
.highlight .mo { color: #666666 } /* Literal.Number.Oct */
.highlight .sa { color: #BA2121 } /* Literal.String.Affix */
.highlight .sb { color: #BA2121 } /* Literal.String.Backtick */
.highlight .sc { color: #BA2121 } /* Literal.String.Char */
.highlight .dl { color: #BA2121 } /* Literal.String.Delimiter */
.highlight .sd { color: #BA2121; font-style: italic } /* Literal.String.Doc */
.highlight .s2 { color: #BA2121 } /* Literal.String.Double */
.highlight .se { color: #BB6622; font-weight: bold } /* Literal.String.Escape */
.highlight .sh { color: #BA2121 } /* Literal.String.Heredoc */
.highlight .si { color: #BB6688; font-weight: bold } /* Literal.String.Interpol */
.highlight .sx { color: #008000 } /* Literal.String.Other */
.highlight .sr { color: #BB6688 } /* Literal.String.Regex */
.highlight .s1 { color: #BA2121 } /* Literal.String.Single */
.highlight .ss { color: #19177C } /* Literal.String.Symbol */
.highlight .bp { color: #008000 } /* Name.Builtin.Pseudo */
.highlight .fm { color: #0000FF } /* Name.Function.Magic */
.highlight .vc { color: #19177C } /* Name.Variable.Class */
.highlight .vg { color: #19177C } /* Name.Variable.Global */
.highlight .vi { color: #19177C } /* Name.Variable.Instance */
.highlight .vm { color: #19177C } /* Name.Variable.Magic */
.highlight .il { color: #666666 } /* Literal.Number.Integer.Long */
    </style>
<style type="text/css">
    
/* Temporary definitions which will become obsolete with Notebook release 5.0 */
.ansi-black-fg { color: #3E424D; }
.ansi-black-bg { background-color: #3E424D; }
.ansi-black-intense-fg { color: #282C36; }
.ansi-black-intense-bg { background-color: #282C36; }
.ansi-red-fg { color: #E75C58; }
.ansi-red-bg { background-color: #E75C58; }
.ansi-red-intense-fg { color: #B22B31; }
.ansi-red-intense-bg { background-color: #B22B31; }
.ansi-green-fg { color: #00A250; }
.ansi-green-bg { background-color: #00A250; }
.ansi-green-intense-fg { color: #007427; }
.ansi-green-intense-bg { background-color: #007427; }
.ansi-yellow-fg { color: #DDB62B; }
.ansi-yellow-bg { background-color: #DDB62B; }
.ansi-yellow-intense-fg { color: #B27D12; }
.ansi-yellow-intense-bg { background-color: #B27D12; }
.ansi-blue-fg { color: #208FFB; }
.ansi-blue-bg { background-color: #208FFB; }
.ansi-blue-intense-fg { color: #0065CA; }
.ansi-blue-intense-bg { background-color: #0065CA; }
.ansi-magenta-fg { color: #D160C4; }
.ansi-magenta-bg { background-color: #D160C4; }
.ansi-magenta-intense-fg { color: #A03196; }
.ansi-magenta-intense-bg { background-color: #A03196; }
.ansi-cyan-fg { color: #60C6C8; }
.ansi-cyan-bg { background-color: #60C6C8; }
.ansi-cyan-intense-fg { color: #258F8F; }
.ansi-cyan-intense-bg { background-color: #258F8F; }
.ansi-white-fg { color: #C5C1B4; }
.ansi-white-bg { background-color: #C5C1B4; }
.ansi-white-intense-fg { color: #A1A6B2; }
.ansi-white-intense-bg { background-color: #A1A6B2; }

.ansi-bold { font-weight: bold; }

    </style>


<style type="text/css">
/* Overrides of notebook CSS for static HTML export */
body {
  overflow: visible;
  padding: 8px;
}

div#notebook {
  overflow: visible;
  border-top: none;
}@media print {
  div.cell {
    display: block;
    page-break-inside: avoid;
  } 
  div.output_wrapper { 
    display: block;
    page-break-inside: avoid; 
  }
  div.output { 
    display: block;
    page-break-inside: avoid; 
  }
}
</style>

<!-- Custom stylesheet, it must be in the same directory as the html file -->
<link rel="stylesheet" href="custom.css">

<!-- Loading mathjax macro -->
<!-- Load mathjax -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS_HTML"></script>
    <!-- MathJax configuration -->
    <script type="text/x-mathjax-config">
    MathJax.Hub.Config({
        tex2jax: {
            inlineMath: [ ['$','$'], ["\\(","\\)"] ],
            displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
            processEscapes: true,
            processEnvironments: true
        },
        // Center justify equations in code and markdown cells. Elsewhere
        // we use CSS to left justify single line equations in code cells.
        displayAlign: 'center',
        "HTML-CSS": {
            styles: {'.MathJax_Display': {"margin": 0}},
            linebreaks: { automatic: true }
        }
    });
    </script>
    <!-- End of mathjax configuration --></head>
<body>
  <div tabindex="-1" id="notebook" class="border-box-sizing">
    <div class="container" id="notebook-container">

<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Maximum-Entropy-Policy-Gradients">Maximum Entropy Policy Gradients<a class="anchor-link" href="#Maximum-Entropy-Policy-Gradients">&#182;</a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>For my first post, I will set up a simple implimentation of maximum entropy policy gradients applied to simple 
grid world problem. Max entropy policy gradients is implemented in a similar manner to vanilla policy gradients,
but includes an added entropy term within the advantage term. For more information see "reinforcement learning and control as probabalistic inference" to see more.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[21]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">torch</span>
<span class="kn">from</span> <span class="nn">copy</span> <span class="k">import</span> <span class="n">deepcopy</span>
<span class="kn">import</span> <span class="nn">warnings</span>
<span class="n">warnings</span><span class="o">.</span><span class="n">filterwarnings</span><span class="p">(</span><span class="s1">&#39;ignore&#39;</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="First-we-want-to-make--a-make-a-policy">First we want to make  a make a policy<a class="anchor-link" href="#First-we-want-to-make--a-make-a-policy">&#182;</a></h2><p>After including our imports we will need to create a policy class to sample new actions, and to compute the 
log probabilities of each of the actions under our current model. I our case, since we will be working with a 
grid world, we will allow 5 actions for up, down, left, right, and stationary. In order to pick one of these 
actions, we will sample from a discrete distribution parameterized by a linear function based on the size of the 
input and the number of actions, and then pass the outputs through a softmax layer. In order to calculate the 
probability of an action, we simply look up the index of the action output by the softmax layer. We also add a little 
bit of noise to the probability to ensure its never exactly zero.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[22]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">class</span> <span class="nc">POLICY</span><span class="p">(</span><span class="n">torch</span><span class="o">.</span><span class="n">nn</span><span class="o">.</span><span class="n">Module</span><span class="p">):</span>
    <span class="sd">&quot;&quot;&quot;</span>
<span class="sd">    POLICY CLASS: CONTAINS ALL USABLE FUNCTIONAL FORMS FOR THE AGENTS POLICY, ALONG WITH</span>
<span class="sd">    THE CORROSPONDING HELPER FUNCTIONS</span>
<span class="sd">    &quot;&quot;&quot;</span>
    <span class="k">def</span> <span class="nf">__init__</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">state_size</span><span class="p">,</span> <span class="n">action_size</span><span class="p">):</span>
        <span class="nb">super</span><span class="p">(</span><span class="n">POLICY</span><span class="p">,</span> <span class="bp">self</span><span class="p">)</span><span class="o">.</span><span class="fm">__init__</span><span class="p">()</span>
        <span class="c1"># initializations</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">state_size</span> <span class="o">=</span> <span class="n">state_size</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">action_size</span> <span class="o">=</span> <span class="n">action_size</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">linear</span> <span class="o">=</span> <span class="n">torch</span><span class="o">.</span><span class="n">nn</span><span class="o">.</span><span class="n">Linear</span><span class="p">(</span><span class="n">state_size</span><span class="p">,</span> <span class="n">action_size</span><span class="p">)</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">softmax</span> <span class="o">=</span> <span class="n">torch</span><span class="o">.</span><span class="n">nn</span><span class="o">.</span><span class="n">Softmax</span><span class="p">()</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">discrete</span> <span class="o">=</span> <span class="k">lambda</span> <span class="n">prob</span><span class="p">:</span> <span class="n">Categorical</span><span class="p">(</span><span class="n">torch</span><span class="o">.</span><span class="n">tensor</span><span class="p">(</span><span class="n">prob</span><span class="p">))</span>

    <span class="k">def</span> <span class="nf">sample_action</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">state</span><span class="p">,</span> <span class="n">randomness</span><span class="p">):</span>
        <span class="c1"># Here the forward pass is simply a linear function</span>
        <span class="n">probabilities</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">linear</span><span class="p">(</span><span class="n">torch</span><span class="o">.</span><span class="n">FloatTensor</span><span class="p">(</span><span class="n">state</span><span class="p">))</span>
        <span class="n">probabilities</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">softmax</span><span class="p">(</span><span class="n">probabilities</span><span class="p">)</span>
        <span class="n">action</span> <span class="o">=</span> <span class="n">torch</span><span class="o">.</span><span class="n">distributions</span><span class="o">.</span><span class="n">Categorical</span><span class="p">(</span><span class="n">probabilities</span><span class="p">)</span>
        
        <span class="c1"># Now check whether we will pick a random action</span>
        <span class="k">if</span> <span class="n">torch</span><span class="o">.</span><span class="n">distributions</span><span class="o">.</span><span class="n">uniform</span><span class="o">.</span><span class="n">Uniform</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mi">1</span><span class="p">)</span><span class="o">.</span><span class="n">sample</span><span class="p">()</span> <span class="o">&lt;</span> <span class="n">randomness</span><span class="p">:</span>
            <span class="k">return</span> <span class="n">torch</span><span class="o">.</span><span class="n">randint</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="mi">5</span><span class="p">,</span> <span class="p">(</span><span class="mi">1</span><span class="p">,))</span>
        <span class="k">else</span><span class="p">:</span>
            <span class="k">return</span> <span class="n">action</span><span class="o">.</span><span class="n">sample</span><span class="p">()</span>

    <span class="k">def</span> <span class="nf">logprob_action</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">state</span><span class="p">,</span> <span class="n">action</span><span class="p">):</span>
        <span class="n">probabilities</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">linear</span><span class="p">(</span><span class="n">state</span><span class="p">)</span>
        <span class="n">probabilities</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">softmax</span><span class="p">(</span><span class="n">probabilities</span><span class="p">)</span>
        <span class="n">log_prob</span> <span class="o">=</span> <span class="nb">min</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="n">torch</span><span class="o">.</span><span class="n">log</span><span class="p">(</span><span class="n">probabilities</span><span class="p">[</span><span class="n">action</span><span class="o">.</span><span class="n">long</span><span class="p">()]</span><span class="o">+</span><span class="mf">0.0001</span><span class="p">))</span>
        <span class="k">return</span> <span class="n">log_prob</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Next-we-need-to-make-the-grid-world-the-agent-will-sample-from">Next we need to make the grid world the agent will sample from<a class="anchor-link" href="#Next-we-need-to-make-the-grid-world-the-agent-will-sample-from">&#182;</a></h2><p>next we need to actually make the simulator thatour agent will be using. The only two functions we will need for
this, one to step the agent, and one to create a full simulations. There are five moves that are possible, and we 
include the abiliity to average over several trajectories.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[23]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">class</span> <span class="nc">GRID_WORLD</span><span class="p">():</span>
    <span class="sd">&quot;&quot;&quot;</span>
<span class="sd">    GRID WORLD CLASS: HELPER CLASS TO INSTANTIATE A SMALL MDP PROBLEM TO TEST POLICY GRADIENT</span>
<span class="sd">    METHODS ON TO ENSURE CORRECTNESS, ALSO ALOWS FOR FOR TESTING OF NEW TYPES OF ENVIROMENTS</span>
<span class="sd">    WITH DIFFERENT STATESPACE DISTRIBUTIONS ECT.</span>
<span class="sd">    &quot;&quot;&quot;</span>

    <span class="k">def</span> <span class="nf">__init__</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">pre_determined_grid</span><span class="p">,</span> <span class="n">start_state</span><span class="p">):</span>
        <span class="nb">super</span><span class="p">(</span><span class="n">GRID_WORLD</span><span class="p">,</span> <span class="bp">self</span><span class="p">)</span><span class="o">.</span><span class="fm">__init__</span><span class="p">()</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">grid</span> <span class="o">=</span> <span class="n">pre_determined_grid</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">start_state</span> <span class="o">=</span> <span class="n">start_state</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">grid_dim</span> <span class="o">=</span> <span class="p">[</span><span class="nb">len</span><span class="p">(</span><span class="n">pre_determined_grid</span><span class="p">),</span><span class="nb">len</span><span class="p">(</span><span class="n">pre_determined_grid</span><span class="p">[</span><span class="mi">0</span><span class="p">])]</span>

    <span class="k">def</span> <span class="nf">step_agent</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">state</span><span class="p">,</span> <span class="n">action</span><span class="p">):</span>
        <span class="c1">#[row, column]</span>
        <span class="k">if</span> <span class="n">action</span> <span class="o">==</span> <span class="mi">1</span><span class="p">:</span> <span class="c1"># down</span>
            <span class="c1"># if we are in top row no change</span>
            <span class="k">if</span> <span class="n">state</span><span class="p">[</span><span class="mi">0</span><span class="p">]</span> <span class="o">==</span> <span class="mi">0</span><span class="p">:</span>
                <span class="kc">None</span>
            <span class="c1"># otherwise increase row position</span>
            <span class="k">else</span><span class="p">:</span>
                <span class="n">state</span><span class="p">[</span><span class="mi">0</span><span class="p">]</span> <span class="o">-=</span> <span class="mi">1</span>
        <span class="k">elif</span> <span class="n">action</span> <span class="o">==</span> <span class="mi">2</span><span class="p">:</span> <span class="c1"># up</span>
            <span class="k">if</span> <span class="n">state</span><span class="p">[</span><span class="mi">0</span><span class="p">]</span> <span class="o">==</span> <span class="bp">self</span><span class="o">.</span><span class="n">grid_dim</span><span class="p">[</span><span class="mi">1</span><span class="p">]</span><span class="o">-</span><span class="mi">1</span><span class="p">:</span>
                <span class="kc">None</span>
            <span class="c1"># otherwise increase row position</span>
            <span class="k">else</span><span class="p">:</span>
                <span class="n">state</span><span class="p">[</span><span class="mi">0</span><span class="p">]</span> <span class="o">+=</span> <span class="mi">1</span>
        <span class="k">elif</span> <span class="n">action</span> <span class="o">==</span> <span class="mi">3</span><span class="p">:</span> <span class="c1"># right</span>
            <span class="k">if</span> <span class="n">state</span><span class="p">[</span><span class="mi">1</span><span class="p">]</span> <span class="o">==</span> <span class="bp">self</span><span class="o">.</span><span class="n">grid_dim</span><span class="p">[</span><span class="mi">0</span><span class="p">]</span><span class="o">-</span><span class="mi">1</span><span class="p">:</span>
                <span class="kc">None</span>
            <span class="c1"># otherwise increase row position</span>
            <span class="k">else</span><span class="p">:</span>
                <span class="n">state</span><span class="p">[</span><span class="mi">1</span><span class="p">]</span> <span class="o">+=</span> <span class="mi">1</span>
        <span class="k">elif</span> <span class="n">action</span> <span class="o">==</span> <span class="mi">4</span><span class="p">:</span> <span class="c1"># down</span>
            <span class="k">if</span> <span class="n">state</span><span class="p">[</span><span class="mi">1</span><span class="p">]</span> <span class="o">==</span> <span class="mi">0</span><span class="p">:</span>
                <span class="kc">None</span>
            <span class="c1"># otherwise increase row position</span>
            <span class="k">else</span><span class="p">:</span>
                <span class="n">state</span><span class="p">[</span><span class="mi">1</span><span class="p">]</span> <span class="o">-=</span> <span class="mi">1</span>
        <span class="k">elif</span> <span class="n">action</span> <span class="o">==</span> <span class="mi">0</span><span class="p">:</span> <span class="c1"># dont_move</span>
            <span class="kc">None</span>
        <span class="k">else</span><span class="p">:</span>
            <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;key error for movement in step_agent&quot;</span><span class="p">)</span>
        <span class="k">return</span> <span class="n">state</span>

    <span class="k">def</span> <span class="nf">simulate_trajectory</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">T</span><span class="p">,</span> <span class="n">samples</span><span class="p">,</span> <span class="n">policy</span><span class="p">):</span>
        <span class="c1"># initialize variables to iterate</span>
        <span class="n">current_state</span> <span class="o">=</span>  <span class="n">deepcopy</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">start_state</span><span class="p">)</span>
        <span class="n">current_reward</span> <span class="o">=</span> <span class="n">deepcopy</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">grid</span><span class="p">[</span><span class="n">current_state</span><span class="p">[</span><span class="mi">0</span><span class="p">]][</span><span class="n">current_state</span><span class="p">[</span><span class="mi">1</span><span class="p">]])</span>
        <span class="n">current_action</span> <span class="o">=</span> <span class="n">policy</span><span class="p">(</span><span class="n">current_state</span><span class="p">)</span>
        <span class="c1"># initialize tensor storage</span>
        <span class="n">state_tensor</span> <span class="o">=</span> <span class="n">torch</span><span class="o">.</span><span class="n">zeros</span><span class="p">((</span><span class="nb">len</span><span class="p">(</span><span class="n">current_state</span><span class="p">),</span> <span class="n">T</span><span class="p">,</span> <span class="n">samples</span><span class="p">),</span> <span class="n">requires_grad</span> <span class="o">=</span> <span class="kc">False</span><span class="p">)</span>
        <span class="n">reward_tensor</span> <span class="o">=</span> <span class="n">torch</span><span class="o">.</span><span class="n">zeros</span><span class="p">((</span><span class="n">T</span><span class="p">,</span> <span class="n">samples</span><span class="p">),</span> <span class="n">requires_grad</span> <span class="o">=</span> <span class="kc">False</span><span class="p">)</span>
        <span class="n">action_tensor</span> <span class="o">=</span> <span class="n">torch</span><span class="o">.</span><span class="n">zeros</span><span class="p">((</span><span class="n">T</span><span class="p">,</span> <span class="n">samples</span><span class="p">),</span> <span class="n">requires_grad</span> <span class="o">=</span> <span class="kc">False</span><span class="p">)</span>
        <span class="c1"># iterate through all states and actions</span>
        <span class="k">for</span> <span class="n">sample</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="n">samples</span><span class="p">):</span>
            <span class="k">for</span> <span class="n">t</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="n">T</span><span class="p">):</span>
                <span class="c1"># store values</span>
                <span class="n">state_tensor</span><span class="p">[:,</span><span class="n">t</span><span class="p">,</span><span class="n">sample</span><span class="p">]</span> <span class="o">=</span> <span class="n">torch</span><span class="o">.</span><span class="n">FloatTensor</span><span class="p">(</span><span class="n">current_state</span><span class="p">)</span>
                <span class="n">reward_tensor</span><span class="p">[</span><span class="n">t</span><span class="p">,</span><span class="n">sample</span><span class="p">]</span> <span class="o">=</span> <span class="n">current_reward</span>
                <span class="n">action_tensor</span><span class="p">[</span><span class="n">t</span><span class="p">,</span><span class="n">sample</span><span class="p">]</span> <span class="o">=</span> <span class="n">current_action</span>
                <span class="c1"># simulate new values</span>
                <span class="n">current_state</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">step_agent</span><span class="p">(</span><span class="n">current_state</span><span class="p">,</span> <span class="n">current_action</span><span class="p">)</span>
                <span class="n">current_reward</span> <span class="o">=</span> <span class="n">deepcopy</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">grid</span><span class="p">[</span><span class="n">current_state</span><span class="p">[</span><span class="mi">0</span><span class="p">]][</span><span class="n">current_state</span><span class="p">[</span><span class="mi">1</span><span class="p">]])</span>
                <span class="n">current_action</span> <span class="o">=</span> <span class="n">policy</span><span class="p">(</span><span class="n">current_state</span><span class="p">)</span>

        <span class="c1"># return it all</span>
        <span class="k">return</span> <span class="n">state_tensor</span><span class="p">,</span> <span class="n">action_tensor</span><span class="p">,</span> <span class="n">reward_tensor</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Now-the-Cost-Function">Now the Cost Function<a class="anchor-link" href="#Now-the-Cost-Function">&#182;</a></h2><p>This cost function will include the primary forward call that is used when we apply pytorches backward function, 
a def to compute the advantage estimate, and finally a baseline function. In this case we could apply a clipped 
exponential average, however this can easily be replaced by a optimization procedure that directly minimizes the 
MSE between the estimated baseline (which is now parameterized), and the cumulative reward (regularized by the entropy). I tried to vectorize as much of the computation as possible without making it unreadable. For now we will 
just leave the baseline at zero, to see if we can stabalize the gradient by increasing sampling.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[24]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">class</span> <span class="nc">POLICY_GRADIENT_LOSS</span><span class="p">(</span><span class="n">torch</span><span class="o">.</span><span class="n">nn</span><span class="o">.</span><span class="n">Module</span><span class="p">):</span>
    <span class="sd">&quot;&quot;&quot; MAXIMUM ENTROPY POLICY GRADIENTS LOSS FUNCTION &quot;&quot;&quot;</span>
    <span class="k">def</span> <span class="nf">__init__</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">alpha</span><span class="p">,</span> <span class="n">discount</span><span class="p">,</span> <span class="n">sd</span><span class="p">,</span> <span class="n">trajectory_length</span><span class="p">,</span> <span class="n">simulations</span><span class="p">):</span>
        <span class="nb">super</span><span class="p">(</span><span class="n">POLICY_GRADIENT_LOSS</span><span class="p">,</span> <span class="bp">self</span><span class="p">)</span><span class="o">.</span><span class="fm">__init__</span><span class="p">()</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">sd</span> <span class="o">=</span> <span class="n">sd</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">alpha</span> <span class="o">=</span> <span class="n">alpha</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">discount</span> <span class="o">=</span> <span class="n">discount</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">trajectory_length</span> <span class="o">=</span> <span class="n">trajectory_length</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">simulations</span> <span class="o">=</span> <span class="n">simulations</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">base_line</span> <span class="o">=</span> <span class="mi">0</span>

    <span class="k">def</span> <span class="nf">Baseline_approximation</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">x</span><span class="p">,</span> <span class="n">y</span><span class="p">,</span> <span class="n">state_size</span><span class="p">):</span>
        <span class="n">baseline</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="k">return</span> <span class="n">baseline</span>

    <span class="k">def</span> <span class="nf">Advantage_estimator</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">logliklihood_tensor</span><span class="p">,</span> <span class="n">trajectories_state_tensor</span><span class="p">,</span> \
                            <span class="n">trajectories_action_tensor</span><span class="p">,</span> <span class="n">trajectories_reward_tensor</span><span class="p">):</span>
        <span class="sd">&quot;&quot;&quot; COMPUTES ROLL OUT WITH MAX ENT REGULARIZATION &quot;&quot;&quot;</span>
        <span class="c1"># initialize cumulative running average for states ahead</span>
        <span class="n">cumulative_rollout</span> <span class="o">=</span> <span class="n">torch</span><span class="o">.</span><span class="n">zeros</span><span class="p">([</span><span class="bp">self</span><span class="o">.</span><span class="n">trajectory_length</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">simulations</span><span class="p">])</span>
        <span class="c1"># calculate cumulative running average for states ahead + subtract entropy term</span>
        <span class="n">cumulative_rollout</span><span class="p">[</span><span class="bp">self</span><span class="o">.</span><span class="n">trajectory_length</span><span class="o">-</span><span class="mi">1</span><span class="p">,:]</span> <span class="o">=</span> <span class="n">trajectories_reward_tensor</span><span class="p">[</span><span class="bp">self</span><span class="o">.</span><span class="n">trajectory_length</span><span class="o">-</span><span class="mi">1</span><span class="p">,:]</span> \
                                                         <span class="o">+</span> <span class="bp">self</span><span class="o">.</span><span class="n">alpha</span><span class="o">*</span><span class="n">logliklihood_tensor</span><span class="p">[</span><span class="bp">self</span><span class="o">.</span><span class="n">trajectory_length</span><span class="o">-</span><span class="mi">1</span><span class="p">,:]</span>
        <span class="c1"># primary loop</span>
        <span class="k">for</span> <span class="n">time</span> <span class="ow">in</span> <span class="nb">reversed</span><span class="p">(</span><span class="nb">range</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">trajectory_length</span><span class="o">-</span><span class="mi">1</span><span class="p">)):</span>
            <span class="n">cumulative_rollout</span><span class="p">[</span><span class="n">time</span><span class="p">,:]</span> <span class="o">=</span> <span class="n">trajectories_reward_tensor</span><span class="p">[</span><span class="n">time</span><span class="p">,</span> <span class="p">:]</span> \
                                         <span class="o">+</span> <span class="bp">self</span><span class="o">.</span><span class="n">discount</span> <span class="o">*</span> <span class="n">cumulative_rollout</span><span class="p">[</span><span class="n">time</span><span class="o">+</span><span class="mi">1</span><span class="p">,:]</span> \
                                         <span class="o">+</span> <span class="bp">self</span><span class="o">.</span><span class="n">alpha</span> <span class="o">*</span> <span class="n">logliklihood_tensor</span><span class="p">[</span><span class="n">time</span><span class="p">,:]</span>
        <span class="c1"># detach cumulative reward from computation graph</span>
        <span class="n">advantage</span> <span class="o">=</span> <span class="n">cumulative_rollout</span><span class="o">.</span><span class="n">detach</span><span class="p">()</span>
        <span class="k">return</span> <span class="n">advantage</span>

    <span class="k">def</span> <span class="nf">forward</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">model_liklihood</span><span class="p">,</span> <span class="n">trajectories_state_tensor</span><span class="p">,</span> <span class="n">trajectories_action_tensor</span><span class="p">,</span> <span class="n">trajectories_reward_tensor</span><span class="p">):</span>
        <span class="sd">&quot;&quot;&quot; CALCULATE LOG LIKLIHOOD OF TRAJECTORIES &quot;&quot;&quot;</span>
        <span class="c1"># initialize tensor for log liklihood stuff</span>
        <span class="n">logliklihood_tensor</span> <span class="o">=</span> <span class="n">torch</span><span class="o">.</span><span class="n">zeros</span><span class="p">([</span><span class="bp">self</span><span class="o">.</span><span class="n">trajectory_length</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">simulations</span><span class="p">])</span>
        <span class="c1"># generate tensor for log liklihood stuff</span>
        <span class="k">for</span> <span class="n">time</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">trajectory_length</span><span class="p">):</span>
            <span class="k">for</span> <span class="n">simulation</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">simulations</span><span class="p">):</span>
                <span class="c1"># [simulation #, value, time step]</span>
                <span class="n">logliklihood_tensor</span><span class="p">[</span><span class="n">time</span><span class="p">,</span><span class="n">simulation</span><span class="p">]</span> <span class="o">=</span> <span class="n">model_liklihood</span><span class="p">(</span><span class="n">trajectories_state_tensor</span><span class="p">[:,</span><span class="n">time</span><span class="p">,</span><span class="n">simulation</span><span class="p">],</span>\
                                                                       <span class="n">trajectories_action_tensor</span><span class="p">[</span><span class="n">time</span><span class="p">,</span><span class="n">simulation</span><span class="p">])</span>
        <span class="sd">&quot;&quot;&quot; CALCULATE ADVANTAGE REGULARIZED BY ENTROPY &quot;&quot;&quot;</span>
        <span class="n">A_hat</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">Advantage_estimator</span><span class="p">(</span><span class="n">logliklihood_tensor</span><span class="p">,</span> <span class="n">trajectories_state_tensor</span><span class="p">,</span> <span class="n">trajectories_action_tensor</span><span class="p">,</span> \
                                         <span class="n">trajectories_reward_tensor</span><span class="p">)</span>
        <span class="sd">&quot;&quot;&quot; CALCULATE POLICY GRADIENT OBJECTIVE &quot;&quot;&quot;</span>
        <span class="c1"># initialize expectation tensor</span>
        <span class="n">expectation</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="c1"># calculate instance of expectation for timestep then calc sample mean</span>
        <span class="k">for</span> <span class="n">time</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">trajectory_length</span><span class="p">):</span>
            <span class="n">expectation</span> <span class="o">+=</span> <span class="n">torch</span><span class="o">.</span><span class="n">dot</span><span class="p">(</span><span class="n">A_hat</span><span class="p">[</span><span class="n">time</span><span class="p">,:],</span> <span class="n">logliklihood_tensor</span><span class="p">[</span><span class="n">time</span><span class="p">,:])</span><span class="o">/</span><span class="bp">self</span><span class="o">.</span><span class="n">simulations</span>
        <span class="c1"># sum accross time</span>
        <span class="n">expectation</span> <span class="o">=</span> <span class="o">-</span><span class="mi">1</span> <span class="o">*</span> <span class="n">torch</span><span class="o">.</span><span class="n">sum</span><span class="p">(</span><span class="n">expectation</span><span class="p">)</span><span class="o">/</span><span class="bp">self</span><span class="o">.</span><span class="n">trajectory_length</span>

        <span class="sd">&quot;&quot;&quot; RETURN &quot;&quot;&quot;</span>
        <span class="k">return</span> <span class="n">expectation</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Lastly-the-Training-Function">Lastly the Training Function<a class="anchor-link" href="#Lastly-the-Training-Function">&#182;</a></h2><p>In this function we generate the policy class, initialize the optimizer and loss functions, and then take optimization 
steps. In order to do this we have to sample a set of trajectories under our current policy, and then compute the entropy regularized cumulative reward at each step. While I have chosen to use Adam, it has been shown that convergence can be sped up by using conjugate gradient decent instead of adams diagnol approximation. In order to do this we would simply average the sum of the outer product of each of our score functions to approximate the fisher information, and then apply a forward solve between this matrix and the gradient vector generated from backprop.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[25]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">train_policy</span><span class="p">(</span><span class="n">epochs</span><span class="p">,</span> <span class="n">grid</span><span class="p">,</span> <span class="n">samples</span><span class="p">,</span> <span class="n">T</span><span class="p">,</span> <span class="n">discount</span><span class="p">,</span> <span class="n">alpha</span><span class="p">,</span> <span class="n">sd</span><span class="p">):</span>

    <span class="c1"># initialize policy</span>
    <span class="n">policy_obj</span> <span class="o">=</span> <span class="n">POLICY</span><span class="p">(</span><span class="mi">2</span><span class="p">,</span> <span class="mi">5</span><span class="p">)</span>

    <span class="c1"># set optimization info</span>
    <span class="n">optimizer</span> <span class="o">=</span> <span class="n">torch</span><span class="o">.</span><span class="n">optim</span><span class="o">.</span><span class="n">Adam</span><span class="p">(</span><span class="n">policy_obj</span><span class="o">.</span><span class="n">parameters</span><span class="p">(),</span> <span class="n">lr</span><span class="o">=</span><span class="mf">1e-2</span><span class="p">)</span>
    <span class="n">loss_func</span> <span class="o">=</span> <span class="n">POLICY_GRADIENT_LOSS</span><span class="p">(</span><span class="n">alpha</span><span class="p">,</span> <span class="n">discount</span><span class="p">,</span> <span class="n">sd</span><span class="p">,</span>  <span class="n">T</span><span class="p">,</span> <span class="n">samples</span><span class="p">)</span>
    
    <span class="c1"># plotting storage (loss and cumulative reward)</span>
    <span class="n">loss_per_iteration</span> <span class="o">=</span> <span class="n">torch</span><span class="o">.</span><span class="n">zeros</span><span class="p">((</span><span class="n">epochs</span><span class="p">))</span>
    <span class="n">cr_per_iteration</span> <span class="o">=</span> <span class="n">torch</span><span class="o">.</span><span class="n">zeros</span><span class="p">((</span><span class="n">epochs</span><span class="p">))</span>
    
    <span class="c1"># train model</span>
    <span class="k">for</span> <span class="n">epoch</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="n">epochs</span><span class="p">):</span>
        
        <span class="c1"># set the probability of picking a random move</span>
        <span class="n">randomness</span> <span class="o">=</span> <span class="p">(</span><span class="mi">10</span> <span class="o">+</span> <span class="n">epoch</span><span class="p">)</span><span class="o">/</span><span class="p">(</span><span class="mi">1</span><span class="o">+</span><span class="n">epoch</span><span class="p">)</span><span class="o">**</span><span class="mi">2</span>
        
        <span class="c1"># set policy</span>
        <span class="n">policy</span> <span class="o">=</span> <span class="k">lambda</span> <span class="n">state</span><span class="p">:</span> <span class="n">policy_obj</span><span class="o">.</span><span class="n">sample_action</span><span class="p">(</span><span class="n">state</span><span class="p">,</span> <span class="n">randomness</span><span class="p">)</span>
        <span class="n">log_prob</span> <span class="o">=</span> <span class="k">lambda</span> <span class="n">state</span><span class="p">,</span> <span class="n">action</span><span class="p">:</span> <span class="n">policy_obj</span><span class="o">.</span><span class="n">logprob_action</span><span class="p">(</span><span class="n">state</span><span class="p">,</span> <span class="n">action</span><span class="p">)</span>
        
        <span class="c1"># get sampled trajectories</span>
        <span class="n">state_tensor</span><span class="p">,</span> <span class="n">action_tensor</span><span class="p">,</span> <span class="n">reward_tensor</span> <span class="o">=</span> <span class="n">grid</span><span class="o">.</span><span class="n">simulate_trajectory</span><span class="p">(</span><span class="n">T</span><span class="p">,</span> <span class="n">samples</span><span class="p">,</span> <span class="n">policy</span><span class="p">)</span>
        
        <span class="c1"># loss objective being minimized</span>
        <span class="n">loss</span> <span class="o">=</span> <span class="n">loss_func</span><span class="p">(</span><span class="n">log_prob</span><span class="p">,</span> <span class="n">state_tensor</span><span class="p">,</span> <span class="n">action_tensor</span><span class="p">,</span> <span class="n">reward_tensor</span><span class="p">)</span>
        
        <span class="c1"># zero the parameter gradients</span>
        <span class="n">optimizer</span><span class="o">.</span><span class="n">zero_grad</span><span class="p">()</span>
        
        <span class="c1"># backprop through computation graph</span>
        <span class="n">loss</span><span class="o">.</span><span class="n">backward</span><span class="p">()</span>
        
        <span class="c1"># step optimizer</span>
        <span class="n">optimizer</span><span class="o">.</span><span class="n">step</span><span class="p">()</span>
        
        <span class="c1"># print loss</span>
        <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;current loss at iteration: &quot;</span> <span class="o">+</span> <span class="nb">str</span><span class="p">(</span><span class="n">epoch</span><span class="o">/</span><span class="n">epochs</span><span class="p">)</span> <span class="o">+</span> <span class="s2">&quot;, with loss: &quot;</span> <span class="o">+</span> <span class="nb">str</span><span class="p">(</span><span class="n">loss</span><span class="p">)</span> <span class="o">+</span> <span class="s2">&quot;, ends in state: &quot;</span> <span class="o">+</span> <span class="nb">str</span><span class="p">(</span><span class="n">state_tensor</span><span class="p">[:,</span><span class="o">-</span><span class="mi">1</span><span class="p">,</span><span class="mi">0</span><span class="p">]),</span> <span class="n">end</span><span class="o">=</span><span class="s1">&#39;</span><span class="se">\r</span><span class="s1">&#39;</span><span class="p">)</span>
        
        <span class="c1"># store loss</span>
        <span class="n">loss_per_iteration</span><span class="p">[</span><span class="n">epoch</span><span class="p">]</span> <span class="o">=</span> <span class="n">loss</span>
        <span class="n">cr_per_iteration</span><span class="p">[</span><span class="n">epoch</span><span class="p">]</span> <span class="o">=</span> <span class="nb">sum</span><span class="p">(</span><span class="nb">sum</span><span class="p">(</span><span class="n">reward_tensor</span><span class="p">))</span><span class="o">/</span><span class="n">samples</span>
        
    <span class="c1"># print a trajectory</span>
    <span class="n">state_tensor</span><span class="p">,</span> <span class="n">action_tensor</span><span class="p">,</span> <span class="n">reward_tensor</span> <span class="o">=</span> <span class="n">grid</span><span class="o">.</span><span class="n">simulate_trajectory</span><span class="p">(</span><span class="n">T</span><span class="p">,</span> <span class="n">samples</span><span class="p">,</span> <span class="n">policy</span><span class="p">)</span>
    <span class="k">for</span> <span class="n">time</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="mi">50</span><span class="p">):</span>
        <span class="nb">print</span><span class="p">(</span><span class="n">state_tensor</span><span class="p">[:,</span><span class="n">time</span><span class="p">,</span><span class="mi">0</span><span class="p">],</span> <span class="n">reward_tensor</span><span class="p">[</span><span class="n">time</span><span class="p">,</span><span class="mi">0</span><span class="p">],</span> <span class="n">action_tensor</span><span class="p">[</span><span class="n">time</span><span class="p">,</span><span class="mi">0</span><span class="p">],</span> <span class="n">end</span><span class="o">=</span><span class="s1">&#39;</span><span class="se">\r</span><span class="s1">&#39;</span><span class="p">)</span>

    <span class="c1"># return policy</span>
    <span class="k">return</span> <span class="n">policy</span><span class="p">,</span> <span class="n">loss_per_iteration</span><span class="p">,</span> <span class="n">cr_per_iteration</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Now-all-we-have-to-do-is-run-it!">Now all we have to do is run it!<a class="anchor-link" href="#Now-all-we-have-to-do-is-run-it!">&#182;</a></h1><p>Below I have set the grid that we will be using, as well as the start state and the number of iterations.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[45]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">pre_determined_grid</span> <span class="o">=</span> <span class="p">[[</span><span class="o">-</span><span class="mi">1</span><span class="p">,</span>   <span class="o">-</span><span class="mi">1</span><span class="p">,</span>  <span class="o">-</span><span class="mi">1</span><span class="p">,</span>   <span class="o">-</span><span class="mi">10</span><span class="p">,</span>   <span class="o">-</span><span class="mi">1</span><span class="p">,</span> <span class="mi">10</span><span class="o">**</span><span class="mi">3</span><span class="p">],</span>
                       <span class="p">[</span><span class="o">-</span><span class="mi">100</span><span class="p">,</span> <span class="o">-</span><span class="mi">1</span><span class="p">,</span>  <span class="o">-</span><span class="mi">10</span><span class="p">,</span>  <span class="o">-</span><span class="mi">1</span><span class="p">,</span>   <span class="o">-</span><span class="mi">10</span><span class="p">,</span> <span class="o">-</span><span class="mi">10</span><span class="p">],</span>
                       <span class="p">[</span><span class="o">-</span><span class="mi">1</span><span class="p">,</span>   <span class="o">-</span><span class="mi">10</span><span class="p">,</span> <span class="o">-</span><span class="mi">100</span><span class="p">,</span> <span class="o">-</span><span class="mi">10</span><span class="p">,</span>  <span class="o">-</span><span class="mi">1</span><span class="p">,</span> <span class="o">-</span><span class="mi">1</span><span class="p">],</span>
                       <span class="p">[</span><span class="o">-</span><span class="mi">1</span><span class="p">,</span>   <span class="o">-</span><span class="mi">1</span><span class="p">,</span>  <span class="o">-</span><span class="mi">100</span><span class="p">,</span> <span class="o">-</span><span class="mi">10</span><span class="p">,</span>  <span class="o">-</span><span class="mi">1</span><span class="p">,</span> <span class="o">-</span><span class="mi">1</span><span class="p">],</span>
                       <span class="p">[</span><span class="o">-</span><span class="mi">10</span><span class="p">,</span>  <span class="o">-</span><span class="mi">1</span><span class="p">,</span>  <span class="o">-</span><span class="mi">100</span><span class="p">,</span> <span class="o">-</span><span class="mi">100</span><span class="p">,</span> <span class="o">-</span><span class="mi">100</span><span class="p">,</span> <span class="o">-</span><span class="mi">1</span><span class="p">],</span>
                       <span class="p">[</span><span class="o">-</span><span class="mi">1</span><span class="p">,</span>   <span class="o">-</span><span class="mi">1</span><span class="p">,</span>  <span class="o">-</span><span class="mi">100</span><span class="p">,</span> <span class="o">-</span><span class="mi">100</span><span class="p">,</span> <span class="o">-</span><span class="mi">100</span><span class="p">,</span> <span class="o">-</span><span class="mi">1</span><span class="p">]]</span>

<span class="c1"># hyper parameters</span>
<span class="n">iterations</span> <span class="o">=</span> <span class="mi">500</span>
<span class="n">Time</span> <span class="o">=</span> <span class="mi">300</span>
<span class="n">simulations</span> <span class="o">=</span> <span class="mi">10</span>
<span class="n">discount</span> <span class="o">=</span> <span class="mf">0.99</span>

<span class="c1">#initializations</span>
<span class="n">start_state</span> <span class="o">=</span> <span class="p">[</span><span class="mi">5</span><span class="p">,</span><span class="mi">0</span><span class="p">]</span>
<span class="n">policy_obj</span> <span class="o">=</span> <span class="n">POLICY</span><span class="p">(</span><span class="mi">2</span><span class="p">,</span> <span class="mi">5</span><span class="p">)</span>
<span class="n">policy</span> <span class="o">=</span> <span class="k">lambda</span> <span class="n">state</span><span class="p">:</span> <span class="n">policy_obj</span><span class="o">.</span><span class="n">sample</span><span class="p">(</span><span class="n">state</span><span class="p">)</span>
<span class="n">grid</span> <span class="o">=</span> <span class="n">GRID_WORLD</span><span class="p">(</span><span class="n">pre_determined_grid</span><span class="p">,</span> <span class="n">start_state</span><span class="p">)</span>
<span class="n">policy</span><span class="p">,</span> <span class="n">loss_per_iteration</span><span class="p">,</span> <span class="n">cr_per_iteration</span> <span class="o">=</span> <span class="n">train_policy</span><span class="p">(</span><span class="n">iterations</span><span class="p">,</span> <span class="n">grid</span><span class="p">,</span> <span class="mi">10</span><span class="p">,</span> <span class="n">Time</span><span class="p">,</span> <span class="n">discount</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>tensor([0., 5.]) tensor(1000.) tensor(3.)ss: tensor(73844.0547, grad_fn=&lt;DivBackward0&gt;), ends in state: tensor([0., 5.])
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Now-lets-plot-everything">Now lets plot everything<a class="anchor-link" href="#Now-lets-plot-everything">&#182;</a></h2>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[47]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">matplotlib.pyplot</span> <span class="k">as</span> <span class="nn">plt</span>
<span class="n">plt</span><span class="o">.</span><span class="n">plot</span><span class="p">([</span><span class="n">i</span> <span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="n">iterations</span><span class="p">)],</span> <span class="o">-</span><span class="mi">1</span><span class="o">*</span><span class="n">loss_per_iteration</span><span class="o">.</span><span class="n">detach</span><span class="p">()</span><span class="o">.</span><span class="n">numpy</span><span class="p">())</span>
<span class="n">plt</span><span class="o">.</span><span class="n">title</span><span class="p">(</span><span class="s1">&#39;Loss Per Iteration&#39;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">ylabel</span><span class="p">(</span><span class="s1">&#39;Loss&#39;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xlabel</span><span class="p">(</span><span class="s1">&#39;Iteration&#39;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>




<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[48]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">plt</span><span class="o">.</span><span class="n">plot</span><span class="p">([</span><span class="n">i</span> <span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="n">iterations</span><span class="p">)],</span> <span class="n">cr_per_iteration</span><span class="o">.</span><span class="n">numpy</span><span class="p">())</span>
<span class="n">plt</span><span class="o">.</span><span class="n">title</span><span class="p">(</span><span class="s1">&#39;Cumulative Reward Per Iteration&#39;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">ylabel</span><span class="p">(</span><span class="s1">&#39;Cumulative Reward&#39;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xlabel</span><span class="p">(</span><span class="s1">&#39;Iteration&#39;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>




<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="It-looks-like-everything-has-converged,-but-lets-look-a-trajectory-to-see-if-we-are-actually-close">It looks like everything has converged, but lets look a trajectory to see if we are actually close<a class="anchor-link" href="#It-looks-like-everything-has-converged,-but-lets-look-a-trajectory-to-see-if-we-are-actually-close">&#182;</a></h2>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[49]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">T</span> <span class="o">=</span> <span class="mi">50</span>
<span class="n">samples</span> <span class="o">=</span> <span class="mi">1</span>
<span class="n">state_tensor</span><span class="p">,</span> <span class="n">action_tensor</span><span class="p">,</span> <span class="n">reward_tensor</span> <span class="o">=</span> <span class="n">grid</span><span class="o">.</span><span class="n">simulate_trajectory</span><span class="p">(</span><span class="n">T</span><span class="p">,</span> <span class="n">samples</span><span class="p">,</span> <span class="n">policy</span><span class="p">)</span>
<span class="k">for</span> <span class="n">time</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="mi">25</span><span class="p">):</span>
    <span class="nb">print</span><span class="p">(</span><span class="n">state_tensor</span><span class="p">[:,</span><span class="n">time</span><span class="p">,</span><span class="mi">0</span><span class="p">],</span> <span class="n">reward_tensor</span><span class="p">[</span><span class="n">time</span><span class="p">,</span><span class="mi">0</span><span class="p">],</span> <span class="n">action_tensor</span><span class="p">[</span><span class="n">time</span><span class="p">,</span><span class="mi">0</span><span class="p">])</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>tensor([5., 0.]) tensor(-1.) tensor(1.)
tensor([4., 0.]) tensor(-10.) tensor(1.)
tensor([3., 0.]) tensor(-1.) tensor(1.)
tensor([2., 0.]) tensor(-1.) tensor(1.)
tensor([1., 0.]) tensor(-100.) tensor(1.)
tensor([0., 0.]) tensor(-1.) tensor(2.)
tensor([1., 0.]) tensor(-100.) tensor(3.)
tensor([1., 1.]) tensor(-1.) tensor(1.)
tensor([0., 1.]) tensor(-1.) tensor(1.)
tensor([0., 1.]) tensor(-1.) tensor(3.)
tensor([0., 2.]) tensor(-1.) tensor(3.)
tensor([0., 3.]) tensor(-10.) tensor(1.)
tensor([0., 3.]) tensor(-10.) tensor(1.)
tensor([0., 3.]) tensor(-10.) tensor(3.)
tensor([0., 4.]) tensor(-1.) tensor(1.)
tensor([0., 4.]) tensor(-1.) tensor(1.)
tensor([0., 4.]) tensor(-1.) tensor(3.)
tensor([0., 5.]) tensor(1000.) tensor(0.)
tensor([0., 5.]) tensor(1000.) tensor(1.)
tensor([0., 5.]) tensor(1000.) tensor(3.)
tensor([0., 5.]) tensor(1000.) tensor(1.)
tensor([0., 5.]) tensor(1000.) tensor(3.)
tensor([0., 5.]) tensor(1000.) tensor(1.)
tensor([0., 5.]) tensor(1000.) tensor(3.)
tensor([0., 5.]) tensor(1000.) tensor(3.)
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="We-can-also-look-at-what-actions-are-sampled-at-all-positions-to-see-if-they-seem-reasonable">We can also look at what actions are sampled at all positions to see if they seem reasonable<a class="anchor-link" href="#We-can-also-look-at-what-actions-are-sampled-at-all-positions-to-see-if-they-seem-reasonable">&#182;</a></h2>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[50]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">for</span> <span class="n">x</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">pre_determined_grid</span><span class="p">[</span><span class="mi">0</span><span class="p">])):</span>
    <span class="n">row</span> <span class="o">=</span> <span class="p">[]</span>
    <span class="k">for</span> <span class="n">y</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">pre_determined_grid</span><span class="p">)):</span>
        <span class="n">row</span><span class="o">.</span><span class="n">append</span><span class="p">(</span><span class="nb">int</span><span class="p">(</span><span class="n">policy</span><span class="p">([</span><span class="n">x</span><span class="p">,</span> <span class="n">y</span><span class="p">])))</span>
    <span class="nb">print</span><span class="p">(</span><span class="n">row</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>[0, 0, 2, 3, 3, 0]
[1, 0, 3, 4, 1, 3]
[3, 0, 1, 1, 1, 0]
[1, 1, 0, 1, 1, 1]
[1, 1, 1, 1, 1, 1]
[1, 1, 1, 1, 1, 1]
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Thats-it-for-my-first-blog-post!">Thats it for my first blog post!<a class="anchor-link" href="#Thats-it-for-my-first-blog-post!">&#182;</a></h2><p>Alright, thats about it! we have a functioning implimentation of maximum entropy policy gradients. Next time we will look at different baseline functions (also called critics) to reduce variance of our score function (the log probability of a action given a state). One thing to note here, is that I did have to fiddle with the reward at the target position to ensure that the agent actually converged to the correct position. In fact for whatever reason, in some cases, even after the addition of some additional noise near the begining of training, the agent settled into not 
doing anything. Which is one of the largest issues with maximum entropy RL, that the ratio of the reward surface and entropy surface matter, and inequity between them can mean bad solutions. Feel free to leave comments or suggestions below to make this code a bit quicker (or if you see any errors). Happy holidays, and Praise the Sun!</p>

</div>
</div>
</div>
    </div>
  </div>
</body>
