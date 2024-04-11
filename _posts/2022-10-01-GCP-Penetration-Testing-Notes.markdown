---
layout: post
title:  GCP Penetration Testing Notes
description: Notes I wrote while reading a blog post written about GCP penetration testing techniques and methodologies by Chris Moberly.
date:   2022-10-01 
image:  '/images/GCP-Penetration-Testing-Notes.jpg'
tags:   [Notes, Privesc, GCP]
---

<html><head><meta http-equiv="Content-Type" content="text/html; charset=utf-8"/><title>GCP Penetration Testing</title>
<style>
.table_of_contents-item {
	display: block;
	font-size: 0.875rem;
	line-height: 1.3;
	padding: 0.125rem;
}

.table_of_contents-indent-1 {
	margin-left: 1.5rem;
}

.table_of_contents-indent-2 {
	margin-left: 3rem;
}

.table_of_contents-indent-3 {
	margin-left: 4.5rem;
}

.table_of_contents-link {
	text-decoration: none;
	opacity: 0.7;
	border-bottom: 1px solid rgba(55, 53, 47, 0.18);
}

table,
th,
td {
	border: 1px solid rgba(55, 53, 47, 0.09);
	border-collapse: collapse;
}

table {
	border-left: none;
	border-right: none;
}

th,
td {
	font-weight: normal;
	padding: 0.25em 0.5em;
	line-height: 1.5;
	min-height: 1.5em;
	text-align: left;
}

th {
	color: rgba(55, 53, 47, 0.6);
}

ol,
ul {
	margin: 0;
	margin-block-start: 0.6em;
	margin-block-end: 0.6em;
}

li > ol:first-child,
li > ul:first-child {
	margin-block-start: 0.6em;
}

ul > li {
	list-style: disc;
}

ul.to-do-list {
	padding-inline-start: 0;
}

ul.to-do-list > li {
	list-style: none;
}

.to-do-children-checked {
	text-decoration: line-through;
	opacity: 0.375;
}

ul.toggle > li {
	list-style: none;
}

ul {
	padding-inline-start: 1.7em;
}

ul > li {
	padding-left: 0.1em;
}

ol {
	padding-inline-start: 1.6em;
}

ol > li {
	padding-left: 0.2em;
}

.mono ol {
	padding-inline-start: 2em;
}

.mono ol > li {
	text-indent: -0.4em;
}

.toggle {
	padding-inline-start: 0em;
	list-style-type: none;
}

/* Indent toggle children */
.toggle > li > details {
	padding-left: 1.7em;
}

.toggle > li > details > summary {
	margin-left: -1.1em;
}

.selected-value {
	display: inline-block;
	padding: 0 0.5em;
	background: rgba(206, 205, 202, 0.5);
	border-radius: 3px;
	margin-right: 0.5em;
	margin-top: 0.3em;
	margin-bottom: 0.3em;
	white-space: nowrap;
}

.collection-title {
	display: inline-block;
	margin-right: 1em;
}

.simple-table {
	margin-top: 1em;
	font-size: 0.875rem;
	empty-cells: show;
}
.simple-table td {
	height: 29px;
	min-width: 120px;
}

.simple-table th {
	height: 29px;
	min-width: 120px;
}

.simple-table-header-color {
	background: rgb(247, 246, 243);
	color: black;
}
.simple-table-header {
	font-weight: 500;
}

time {
	opacity: 0.5;
}

.icon {
	display: inline-block;
	max-width: 1.2em;
	max-height: 1.2em;
	text-decoration: none;
	vertical-align: text-bottom;
	margin-right: 0.5em;
}

img.icon {
	border-radius: 3px;
}

.user-icon {
	width: 1.5em;
	height: 1.5em;
	border-radius: 100%;
	margin-right: 0.5rem;
}

.user-icon-inner {
	font-size: 0.8em;
}

.text-icon {
	border: 1px solid #000;
	text-align: center;
}

.page-cover-image {
	display: block;
	object-fit: cover;
	width: 100%;
	max-height: 30vh;
}

.page-header-icon {
	font-size: 3rem;
	margin-bottom: 1rem;
}

.page-header-icon-with-cover {
	margin-top: -0.72em;
	margin-left: 0.07em;
}

.page-header-icon img {
	border-radius: 3px;
}

.link-to-page {
	margin: 1em 0;
	padding: 0;
	border: none;
	font-weight: 500;
}

p > .user {
	opacity: 0.5;
}

td > .user,
td > time {
	white-space: nowrap;
}

input[type="checkbox"] {
	transform: scale(1.5);
	margin-right: 0.6em;
	vertical-align: middle;
}

p {
	margin-top: 0.5em;
	margin-bottom: 0.5em;
}

.image {
	border: none;
	margin: 1.5em 0;
	padding: 0;
	border-radius: 0;
	text-align: center;
}

.code,
code {
	background: rgba(135, 131, 120, 0.15);
	border-radius: 3px;
	padding: 0.2em 0.4em;
	border-radius: 3px;
	font-size: 85%;
	tab-size: 2;
}

code {
	color: #eb5757;
}

.code {
	padding: 1.5em 1em;
}

.code-wrap {
	white-space: pre-wrap;
	word-break: break-all;
}

.code > code {
	background: none;
	padding: 0;
	font-size: 100%;
	color: inherit;
}

blockquote {
	font-size: 1.25em;
	margin: 1em 0;
	padding-left: 1em;
	border-left: 3px solid rgb(55, 53, 47);
}

.bookmark {
	text-decoration: none;
	max-height: 8em;
	padding: 0;
	display: flex;
	width: 100%;
	align-items: stretch;
}

.bookmark-title {
	font-size: 0.85em;
	overflow: hidden;
	text-overflow: ellipsis;
	height: 1.75em;
	white-space: nowrap;
}

.bookmark-text {
	display: flex;
	flex-direction: column;
}

.bookmark-info {
	flex: 4 1 180px;
	padding: 12px 14px 14px;
	display: flex;
	flex-direction: column;
	justify-content: space-between;
}

.bookmark-image {
	width: 33%;
	flex: 1 1 180px;
	display: block;
	position: relative;
	object-fit: cover;
	border-radius: 1px;
}

.bookmark-description {
	color: rgba(55, 53, 47, 0.6);
	font-size: 0.75em;
	overflow: hidden;
	max-height: 4.5em;
	word-break: break-word;
}

.bookmark-href {
	font-size: 0.75em;
	margin-top: 0.25em;
}

.sans { font-family: ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, "Apple Color Emoji", Arial, sans-serif, "Segoe UI Emoji", "Segoe UI Symbol"; }
.code { font-family: "SFMono-Regular", Menlo, Consolas, "PT Mono", "Liberation Mono", Courier, monospace; }
.serif { font-family: Lyon-Text, Georgia, ui-serif, serif; }
.mono { font-family: iawriter-mono, Nitti, Menlo, Courier, monospace; }
.pdf .sans { font-family: Inter, ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, "Apple Color Emoji", Arial, sans-serif, "Segoe UI Emoji", "Segoe UI Symbol", 'Twemoji', 'Noto Color Emoji', 'Noto Sans CJK JP'; }
.pdf:lang(zh-CN) .sans { font-family: Inter, ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, "Apple Color Emoji", Arial, sans-serif, "Segoe UI Emoji", "Segoe UI Symbol", 'Twemoji', 'Noto Color Emoji', 'Noto Sans CJK SC'; }
.pdf:lang(zh-TW) .sans { font-family: Inter, ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, "Apple Color Emoji", Arial, sans-serif, "Segoe UI Emoji", "Segoe UI Symbol", 'Twemoji', 'Noto Color Emoji', 'Noto Sans CJK TC'; }
.pdf:lang(ko-KR) .sans { font-family: Inter, ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, "Apple Color Emoji", Arial, sans-serif, "Segoe UI Emoji", "Segoe UI Symbol", 'Twemoji', 'Noto Color Emoji', 'Noto Sans CJK KR'; }
.pdf .code { font-family: Source Code Pro, "SFMono-Regular", Menlo, Consolas, "PT Mono", "Liberation Mono", Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK JP'; }
.pdf:lang(zh-CN) .code { font-family: Source Code Pro, "SFMono-Regular", Menlo, Consolas, "PT Mono", "Liberation Mono", Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK SC'; }
.pdf:lang(zh-TW) .code { font-family: Source Code Pro, "SFMono-Regular", Menlo, Consolas, "PT Mono", "Liberation Mono", Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK TC'; }
.pdf:lang(ko-KR) .code { font-family: Source Code Pro, "SFMono-Regular", Menlo, Consolas, "PT Mono", "Liberation Mono", Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK KR'; }
.pdf .serif { font-family: PT Serif, Lyon-Text, Georgia, ui-serif, serif, 'Twemoji', 'Noto Color Emoji', 'Noto Serif CJK JP'; }
.pdf:lang(zh-CN) .serif { font-family: PT Serif, Lyon-Text, Georgia, ui-serif, serif, 'Twemoji', 'Noto Color Emoji', 'Noto Serif CJK SC'; }
.pdf:lang(zh-TW) .serif { font-family: PT Serif, Lyon-Text, Georgia, ui-serif, serif, 'Twemoji', 'Noto Color Emoji', 'Noto Serif CJK TC'; }
.pdf:lang(ko-KR) .serif { font-family: PT Serif, Lyon-Text, Georgia, ui-serif, serif, 'Twemoji', 'Noto Color Emoji', 'Noto Serif CJK KR'; }
.pdf .mono { font-family: PT Mono, iawriter-mono, Nitti, Menlo, Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK JP'; }
.pdf:lang(zh-CN) .mono { font-family: PT Mono, iawriter-mono, Nitti, Menlo, Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK SC'; }
.pdf:lang(zh-TW) .mono { font-family: PT Mono, iawriter-mono, Nitti, Menlo, Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK TC'; }
.pdf:lang(ko-KR) .mono { font-family: PT Mono, iawriter-mono, Nitti, Menlo, Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK KR'; }
.highlight-default {
	color: rgba(55, 53, 47, 1);
}
.highlight-gray {
	color: rgba(120, 119, 116, 1);
	fill: rgba(120, 119, 116, 1);
}
.highlight-brown {
	color: rgba(159, 107, 83, 1);
	fill: rgba(159, 107, 83, 1);
}
.highlight-orange {
	color: rgba(217, 115, 13, 1);
	fill: rgba(217, 115, 13, 1);
}
.highlight-yellow {
	color: rgba(203, 145, 47, 1);
	fill: rgba(203, 145, 47, 1);
}
.highlight-teal {
	color: rgba(68, 131, 97, 1);
	fill: rgba(68, 131, 97, 1);
}
.highlight-blue {
	color: rgba(51, 126, 169, 1);
	fill: rgba(51, 126, 169, 1);
}
.highlight-purple {
	color: rgba(144, 101, 176, 1);
	fill: rgba(144, 101, 176, 1);
}
.highlight-pink {
	color: rgba(193, 76, 138, 1);
	fill: rgba(193, 76, 138, 1);
}
.highlight-red {
	color: rgba(212, 76, 71, 1);
	fill: rgba(212, 76, 71, 1);
}
.highlight-gray_background {
	background: rgba(241, 241, 239, 1);
}
.highlight-brown_background {
	background: rgba(244, 238, 238, 1);
}
.highlight-orange_background {
	background: rgba(251, 236, 221, 1);
}
.highlight-yellow_background {
	background: rgba(251, 243, 219, 1);
}
.highlight-teal_background {
	background: rgba(237, 243, 236, 1);
}
.highlight-blue_background {
	background: rgba(231, 243, 248, 1);
}
.highlight-purple_background {
	background: rgba(244, 240, 247, 0.8);
}
.highlight-pink_background {
	background: rgba(249, 238, 243, 0.8);
}
.highlight-red_background {
	background: rgba(253, 235, 236, 1);
}
.block-color-default {
	color: inherit;
	fill: inherit;
}
.block-color-gray {
	color: rgba(120, 119, 116, 1);
	fill: rgba(120, 119, 116, 1);
}
.block-color-brown {
	color: rgba(159, 107, 83, 1);
	fill: rgba(159, 107, 83, 1);
}
.block-color-orange {
	color: rgba(217, 115, 13, 1);
	fill: rgba(217, 115, 13, 1);
}
.block-color-yellow {
	color: rgba(203, 145, 47, 1);
	fill: rgba(203, 145, 47, 1);
}
.block-color-teal {
	color: rgba(68, 131, 97, 1);
	fill: rgba(68, 131, 97, 1);
}
.block-color-blue {
	color: rgba(51, 126, 169, 1);
	fill: rgba(51, 126, 169, 1);
}
.block-color-purple {
	color: rgba(144, 101, 176, 1);
	fill: rgba(144, 101, 176, 1);
}
.block-color-pink {
	color: rgba(193, 76, 138, 1);
	fill: rgba(193, 76, 138, 1);
}
.block-color-red {
	color: rgba(212, 76, 71, 1);
	fill: rgba(212, 76, 71, 1);
}
.block-color-gray_background {
	background: rgba(241, 241, 239, 1);
}
.block-color-brown_background {
	background: rgba(244, 238, 238, 1);
}
.block-color-orange_background {
	background: rgba(251, 236, 221, 1);
}
.block-color-yellow_background {
	background: rgba(251, 243, 219, 1);
}
.block-color-teal_background {
	background: rgba(237, 243, 236, 1);
}
.block-color-blue_background {
	background: rgba(231, 243, 248, 1);
}
.block-color-purple_background {
	background: rgba(244, 240, 247, 0.8);
}
.block-color-pink_background {
	background: rgba(249, 238, 243, 0.8);
}
.block-color-red_background {
	background: rgba(253, 235, 236, 1);
}
.select-value-color-pink { background-color: rgba(245, 224, 233, 1); }
.select-value-color-purple { background-color: rgba(232, 222, 238, 1); }
.select-value-color-green { background-color: rgba(219, 237, 219, 1); }
.select-value-color-gray { background-color: rgba(227, 226, 224, 1); }
.select-value-color-opaquegray { background-color: rgba(255, 255, 255, 0.0375); }
.select-value-color-orange { background-color: rgba(250, 222, 201, 1); }
.select-value-color-brown { background-color: rgba(238, 224, 218, 1); }
.select-value-color-red { background-color: rgba(255, 226, 221, 1); }
.select-value-color-yellow { background-color: rgba(253, 236, 200, 1); }
.select-value-color-blue { background-color: rgba(211, 229, 239, 1); }

.checkbox {
	display: inline-flex;
	vertical-align: text-bottom;
	width: 16;
	height: 16;
	background-size: 16px;
	margin-left: 2px;
	margin-right: 5px;
}

.checkbox-on {
	background-image: url("data:image/svg+xml;charset=UTF-8,%3Csvg%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%0A%3Crect%20width%3D%2216%22%20height%3D%2216%22%20fill%3D%22%2358A9D7%22%2F%3E%0A%3Cpath%20d%3D%22M6.71429%2012.2852L14%204.9995L12.7143%203.71436L6.71429%209.71378L3.28571%206.2831L2%207.57092L6.71429%2012.2852Z%22%20fill%3D%22white%22%2F%3E%0A%3C%2Fsvg%3E");
}

.checkbox-off {
	background-image: url("data:image/svg+xml;charset=UTF-8,%3Csvg%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%0A%3Crect%20x%3D%220.75%22%20y%3D%220.75%22%20width%3D%2214.5%22%20height%3D%2214.5%22%20fill%3D%22white%22%20stroke%3D%22%2336352F%22%20stroke-width%3D%221.5%22%2F%3E%0A%3C%2Fsvg%3E");
}
	
</style>
The PDF version of these notes can be found <a href="https://0xd4y.com/misc/GCP_Penetration_Testing_Notes.pdf">here.</a>
</head><body><article id="0e09bf4e-83fa-497c-ba80-4234eeac74c7" class="page sans"><header><img class="page-cover-image" src="../../../../images/GCP%20Penetration%20Testing%200e09bf4e83fa497cba804234eeac74c7/Notes_Cover.png" style="object-position:center 50%"/><h1 class="page-title">GCP Penetration Testing</h1></header><div class="page-body"><h2 id="db1fada2-639f-4e82-8e71-11bd9da26a51" class="">Contact</h2><p id="9806862e-8ca4-4e1d-8315-ba9640bc8ce7" class="">LinkedIn: <a href="https://www.linkedin.com/in/segev-eliezer/">https://www.linkedin.com/in/segev-eliezer/</a></p><p id="9b4ce2d5-3678-43c8-94a2-be506da78c74" class="">YouTube: <a href="https://YouTube.com/@0xd4y">https://YouTube.com/@0xd4y</a> </p><p id="ea2b847d-ba06-453a-a885-8a507befdc22" class="">GitHub: <a href="https://github.com/0xd4y">https://github.com/0xd4y</a></p><h2 id="4584475f-cdf2-4914-bfb9-c1e757acfeea" class="">Table of Contents</h2><nav id="430dec82-5f1e-4fce-9e86-0aeb7ba652f8" class="block-color-gray table_of_contents"><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#db1fada2-639f-4e82-8e71-11bd9da26a51">Contact</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#4584475f-cdf2-4914-bfb9-c1e757acfeea">Table of Contents</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#b000bb23-7d65-4423-9678-d3bf3ac3162a">Privesc and Post-Exploitation in GCP by <strong>Chris Moberly</strong></a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#7790deb9-e5b7-4f63-a889-9ccbdda53449">GCP Fundamentals</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#3d47c9b8-8e0c-4c8b-a56f-fdda7dd2c982">Service Accounts</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#bc9a4211-de81-409d-9e81-3361a0aca5a4">Access Scopes</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#fae2701f-5a94-47a8-9d81-fac7280cfc0b">IAM</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#43d3a41e-e1b5-4b30-b28d-5af33fc071fe">Enumeration</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#95d8bafd-6db2-40a0-8949-fa905e3a2ddd">Application Default Credentials</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#cd5491d4-a7aa-4137-8457-f47910e1860e">Service Account Token</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#0beade45-8237-49d9-85a9-b6ef48010164">Application Default Credentials</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#a719417a-8f16-429e-9c45-e60f8d71be66">Privilege Escalation</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#a5272a0b-1ad9-4f45-8a04-3bea0b6d9a8a">SSRF</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#ec4b0bd1-0559-4653-b53e-ba35f93abfdd">Insecure Metadata Endpoint</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#e8a13e55-0dab-4e9a-b849-78997fd520af">Compute Instances</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#97607a87-1f35-4a78-babb-32c9e59a51a7">General</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#05544736-8d54-4d56-bbaf-c6e38ecabe2a">Modifying Instance Metadata</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#4bd8db62-b0a5-441f-b5f4-9c74ddcf154d">Bypassing Access Scopes</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#79c32afa-c47f-4da8-b145-aae2a586e1fd">Steal GCloud Authorizations</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#4e0f8397-fc1f-4b6e-b0ac-32f8ee4d74d0">Service Account Impersonation</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#7aecfdcf-4ac6-4fce-a482-fe8b6c41475a">Accessing Databases</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#9dfe8ef8-31ff-416a-b740-0a72e8b5b343">Storage Buckets</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#956083e1-02f0-4fb5-89eb-b5776b45fae0">Decrypting Secrets</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#1dae6180-a121-4a1c-85c5-c45a0ab2c909">Enumeration</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#7b4dfdcd-35b8-4be4-98b8-cde58a376286">Serial Console Logs</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#e0452e20-dd37-48b1-8bf9-1780b2736d85">Custom Images</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#4bf0e21b-a818-4a12-bf3a-7df8bf72e416">Custom Templates</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#fea88e71-c5e6-4e2a-a2a7-59d282598fa0">StackDriver Logging</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#1be71d6b-5fcb-4d02-a107-7255fc0623cc">Serverless Services</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#95da81fb-cb97-4bd4-bd91-5824aa948fa1">Cloud Functions</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#1a337c12-6474-4c97-8b7d-9710806866ab">App Engine</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#bd9ba57d-bbda-45b4-8309-6a7cebd4790c">Cloud Run</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#8f7cbd77-79f6-4a50-978a-f4bbd8fc90e9">AI Platform</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#39e69c66-6575-480e-883b-a8d79dfac461">Cloud Pub/Sub</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#cc055ada-5549-4a52-9bd4-530cb28ef0ca">Cloud Source Repos</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#0c2c99c5-9105-49ff-8b38-3b8f36a11a61">Cloud Filestore</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#0e2f40d3-0aa7-4d9e-a3a8-2eda4beffb79">Kubernetes</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#f63c53d2-e183-4a49-9108-d8ef81f19659">Secrets Management</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#dfd7ccbc-957f-400d-97ae-9a8fce7b3b0b">Local System Secrets</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#5f5e2e92-c686-47d9-ad62-3c224f6928e1">Networking</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#53132471-6e8e-4578-82de-5beee1c6d0c3">Firewall</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#e710fc28-94e2-474c-bb1d-6ee5b59f4356">Enumeration</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#7472f727-17ef-485b-a091-d66957103b2c">G Suite</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#0f328051-bce2-4ad7-b0dc-13a361d03aee">Authenticating to G Suite</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#655bc3a8-ee95-443b-9d2c-bd0d814732f2">Tools</a></div></nav><h1 id="b000bb23-7d65-4423-9678-d3bf3ac3162a" class="">Privesc and Post-Exploitation in GCP by <strong>Chris Moberly</strong></h1><p id="89eef233-a47f-4a07-ab3c-cc366dc5c15d" class=""><a href="https://about.gitlab.com/blog/2020/02/12/plundering-gcp-escalating-privileges-in-google-cloud-platform/">https://about.gitlab.com/blog/2020/02/12/plundering-gcp-escalating-privileges-in-google-cloud-platform/</a></p><p id="a9d328ac-d294-469f-9052-a4a7d7852a04" class="">
</p><h1 id="7790deb9-e5b7-4f63-a889-9ccbdda53449" class="">GCP Fundamentals</h1><ul id="65b954e6-672d-4b93-b453-7154d46800c1" class="bulleted-list"><li style="list-style-type:disc">raw HTTP API call for a given <code>gcloud</code> command can be found by appending <code>--log-http</code> to the command</li></ul><p id="f276df9f-a31d-4764-9df9-6af4f204ac38" class=""><strong>Recursively enumerate an instance’s metadata:</strong></p><pre id="2ccf56d0-e9ad-428c-be8f-d71d3dad8ec2" class="code"><code>curl &quot;http://metadata.google.internal/computeMetadata/v1/?recursive=true&amp;alt=text&quot; -H &quot;Metadata-Flavor: Google&quot;
</code></pre><ul id="f45db67e-ed9d-46a6-b39c-30077bf4655b" class="bulleted-list"><li style="list-style-type:disc">you may find some juicy information in the metadata including private SSH keys</li></ul><p id="f67e1e42-ff90-43b8-ac93-71fcef627adc" class="">
</p><ul id="a14c6c4e-6320-479f-a15e-1c60e9b51641" class="bulleted-list"><li style="list-style-type:disc">GCP uses a resource hierarchy<ul id="85f79bc1-d8a4-4049-aeb8-538ae890a573" class="bulleted-list"><li style="list-style-type:circle">similar to traditional filesystem structure:</li></ul><pre id="68a5e4cf-2519-45a2-bb35-c80031235af1" class="code"><code>Organization
--&gt; Folders
  --&gt; Projects
    --&gt; Resources</code></pre><ul id="01e9274e-fc53-447d-af62-d001aed836c8" class="bulleted-list"><li style="list-style-type:circle">therefore, if a user has a certain permission to an organization, that permission gets propagated to folders, projects, and resources</li></ul></li></ul><h2 id="3d47c9b8-8e0c-4c8b-a56f-fdda7dd2c982" class="">Service Accounts</h2><ul id="93bacd49-758b-4f69-9af6-1f726f91ed91" class="bulleted-list"><li style="list-style-type:disc">every GCP project has a default service account<ul id="2d62205d-e04d-4e85-8e34-57cb2b1d3a37" class="bulleted-list"><li style="list-style-type:circle">this service account gets assigned to any resource created within that project as well</li></ul></li></ul><p id="e443a35c-ad2d-4035-b1fc-0fd3f6aa274f" class="">Default service accounts look like the following:</p><pre id="844663f1-f12f-4e4b-93da-adf35a73c754" class="code"><code>PROJECT_NUMBER-compute@developer.gserviceaccount.com
PROJECT_ID@appspot.gserviceaccount.com</code></pre><p id="713a61e5-0faf-4f1a-8e71-cdc695f50db1" class="">Custom service accounts look like the following:</p><pre id="ae84d272-ded7-46e1-89aa-20f3c4169f52" class="code"><code>SERVICE_ACCOUNT_NAME@PROJECT_NAME.iam.gserviceaccount.com</code></pre><h3 id="bc9a4211-de81-409d-9e81-3361a0aca5a4" class="">Access Scopes</h3><ul id="55f5ee86-417c-42eb-9061-e426f1f094b8" class="bulleted-list"><li style="list-style-type:disc">the access scope of a service account can be seen by querying the <code>169.254.169.254</code> IP such as in the example below:</li></ul><pre id="1b1b5a59-d8f5-403e-a19e-fae5aaebd4d1" class="code"><code>$ curl http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default/scopes \
    -H &#x27;Metadata-Flavor:Google&#x27;

https://www.googleapis.com/auth/devstorage.read_only
https://www.googleapis.com/auth/logging.write
https://www.googleapis.com/auth/monitoring.write
https://www.googleapis.com/auth/servicecontrol
https://www.googleapis.com/auth/service.management.readonly
https://www.googleapis.com/auth/trace.append</code></pre><ul id="1f631303-919c-4e68-836e-229a6ea22efc" class="bulleted-list"><li style="list-style-type:disc">the <code>devstorage.read_only</code> default scope allows read access to all storage buckets within the specified project</li></ul><ul id="d70d9b7a-08e0-476c-95b0-6eddfe6df5de" class="bulleted-list"><li style="list-style-type:disc">access scopes should not be relied on as a boundary for a service account’s permissions</li></ul><ul id="6dea22a1-e6cb-4fc9-a4ab-7ec4820098d8" class="bulleted-list"><li style="list-style-type:disc"> when <code>cloud-platform</code> is specified for an instance, the service account can attempt to authenticate to all API endpoints <ul id="b655ce3d-d15e-4b71-afd3-af4bed9fef6d" class="bulleted-list"><li style="list-style-type:circle">this authentication will be successful if the permissions of the storage account allow it</li></ul></li></ul><ul id="bc8b860d-5660-494c-b2cc-4872d9891b0a" class="bulleted-list"><li style="list-style-type:disc">even though a service account may have permissions to access a certain API endpoint, if this endpoint is not allowed by the access scope, successful authentication cannot occur</li></ul><h1 id="fae2701f-5a94-47a8-9d81-fac7280cfc0b" class="">IAM</h1><p id="deaefd91-5d5b-477b-9793-4c200e8a4f66" class=""><span style="border-bottom:0.05em solid">Primitive roles</span> </p><ul id="af79edca-5ce8-4b19-bf4f-5c056235fd80" class="bulleted-list"><li style="list-style-type:disc"><code>Owner</code>, <code>Editor</code>, and <code>Viewer</code></li></ul><ul id="c784682a-76a7-4435-a1af-9998ebcd16a5" class="bulleted-list"><li style="list-style-type:disc">!!!! default service account in every project is given the <code>Editor</code> role (insecure!!)</li></ul><p id="4bb41c6d-dfdc-4a99-97a3-e5f4cc7afd03" class=""><span style="border-bottom:0.05em solid">Predefined roles</span></p><ul id="52c947fd-3d64-4d1b-9e00-b80b74421bb6" class="bulleted-list"><li style="list-style-type:disc">roles managed by Google (e.g. <code>compute.instanceAdmin</code>) </li></ul><p id="5f104916-f684-4f60-a9a0-5aa48b8366c6" class=""><span style="border-bottom:0.05em solid">Custom roles</span></p><ul id="9187500e-10b3-489a-b4c2-b3f359b40a7e" class="bulleted-list"><li style="list-style-type:disc">provides admins the ability to create their own set of permissions for a role</li></ul><p id="0a1f503d-768a-4b29-bac5-e4083d09b5f1" class="">To see roles assigned to each member of a project:</p><p id="c1f45d40-6b54-4b6f-a51e-85fad7f604d0" class=""><code>gcloud projects get-iam-policy &lt;PROJECT_ID&gt;</code></p><h2 id="43d3a41e-e1b5-4b30-b28d-5af33fc071fe" class="">Enumeration</h2><table id="4755a6df-d813-4486-b93c-2dcab1446344" class="simple-table"><tbody><tr id="a1ba3cc0-b5d6-4fa0-9f03-528dce8d97b4"><td id="q;W{" class="" style="width:306px"><strong>Command</strong></td><td id="YRRn" class="" style="width:396px"><strong>Description</strong></td></tr><tr id="76a4cc8e-a328-47e8-b8d4-246be504dc2f"><td id="q;W{" class="" style="width:306px"><code>gcloud organizations list</code></td><td id="YRRn" class="" style="width:396px">Get organization ID</td></tr><tr id="879c0b7c-a5c2-460f-adf4-34b792c64c24"><td id="q;W{" class="" style="width:306px"><code>gcloud organizations get-iam-policy</code></td><td id="YRRn" class="" style="width:396px">View user permissions within organization</td></tr></tbody></table><ul id="e95d3ac6-a5ed-44cb-a345-8e8bc0a65f4b" class="bulleted-list"><li style="list-style-type:disc">note that the permissions within an organization are applied to all projects within the organization, which are therefore applied to all resources within that project, etc.</li></ul><h1 id="95d8bafd-6db2-40a0-8949-fa905e3a2ddd" class="">Application Default Credentials</h1><h2 id="cd5491d4-a7aa-4137-8457-f47910e1860e" class="">Service Account Token</h2><p id="e37034a3-1dea-42df-ab6b-304295f76469" class="">Token can be retrieved from metadata service:</p><p id="9149445f-10dd-480d-b1f7-62fabf22e2c2" class=""><strong>Request</strong></p><pre id="a4a14730-4f51-4796-838e-8ed52d3a3be5" class="code"><code>curl &quot;http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default/token&quot; -H &quot;Metadata-Flavor: Google&quot;</code></pre><p id="3f3ba5d3-7293-4407-9729-2c76e2fb357a" class=""><strong>Response</strong></p><pre id="bedc0c95-c9b1-4e4d-99f7-50aa242c8ac2" class="code code-wrap"><code>{
      &quot;access_token&quot;:&quot;ya29.AHES6ZRN3-HlhAPya30GnW_bHSb_QtAS08i85nHq39HE3C2LTrCARA&quot;,
      &quot;expires_in&quot;:3599,
      &quot;token_type&quot;:&quot;Bearer&quot;
 }</code></pre><h2 id="0beade45-8237-49d9-85a9-b6ef48010164" class="">Application Default Credentials</h2><ul id="ea4d64e1-d6c2-4b94-978e-c3e3b8983e2e" class="bulleted-list"><li style="list-style-type:disc">alternative to pulling a token from the metadata service<ul id="d9294d78-ad12-49a8-9a07-60513807a454" class="bulleted-list"><li style="list-style-type:circle">this method is used when implementing one of Google’s official GCP client libraries</li></ul></li></ul><p id="b3890d19-e557-4ed7-bb8a-3b8e1fe6c08d" class="">The following are the steps taken to search for credentials when using the GCP client libraries:</p><ol type="1" id="d39a7cba-8ae7-4dfd-b481-38352c08a1d1" class="numbered-list" start="1"><li>Code will check source code<ol type="a" id="8f346a56-d8ff-43ef-a520-5addf46f1b2a" class="numbered-list" start="1"><li>The service account key file is checked</li></ol></li></ol><ol type="1" id="12a99fd6-e7a4-4e76-8e98-845064eb7119" class="numbered-list" start="2"><li>The <code>GOOGLE_APPLICATION_CREDENTIALS</code> environment variable is checked<ol type="a" id="5c3319c6-a2f5-43e5-b07d-9c128aa17334" class="numbered-list" start="1"><li>This environment variable can be set to the location of a service account key file</li></ol></li></ol><ol type="1" id="8ff4e0eb-d36b-4a48-8e96-9bd3dc6f2e32" class="numbered-list" start="3"><li>The default token in the metadata service is used.</li></ol><ul id="e40a8444-ebdf-4249-88a5-267b6107ec25" class="bulleted-list"><li style="list-style-type:disc">the default token in the metadata service is used only if 1 or 2 is not found because the metadata service token is confined within access scopes and is temporary</li></ul><h1 id="a719417a-8f16-429e-9c45-e60f8d71be66" class="">Privilege Escalation</h1><p id="2b466abc-d67c-4789-85dd-18b0d1dd7ee2" class="">⭐ Always make sure to check if the principle of least-privilege is being applied throughout the environment</p><h2 id="a5272a0b-1ad9-4f45-8a04-3bea0b6d9a8a" class="">SSRF</h2><p id="c8fb0917-af80-47c0-93f0-be2cd14e4af1" class="">The privesc techniques described below are written from the perspective of internal access to a compromised instance. However, they can also be performed if you find SSRF in some cases.</p><h3 id="ec4b0bd1-0559-4653-b53e-ba35f93abfdd" class="">Insecure Metadata Endpoint</h3><p id="0bca4390-7d97-4855-86e1-462f73d17548" class="">If the client has a <code>/v1beta</code> enabled, you can get the access token without the special header:</p><pre id="82dc2dae-daf0-4df3-acf7-c18265e402de" class="code"><code>curl http://metadata.google.internal/computeMetadata/v1beta/instance/service-accounts/default/token</code></pre><p id="27e05add-8a0d-4a30-9521-442727ae68ba" class="">If you have SSRF, check if the gopher protocol is enabled. It may be possible to query the metadata service with<code>curl gopher://metadata.google.internal:80/xGET%2520/computeMetadata/v1/instance/%2520HTTP%252f%2531%252e%2531%250AHost:%2520metadata.google.internal%250AAccept:%2520%252a%252f%252a%250aMetadata-Flavor:%2520Google%250d%250a</code><br>Otherwise, you must query <code><a href="http://metadata.google.internal/computeMetadata/v1beta/instance/service-accounts/default/token">http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default/token</a></code> with a custom header set</p><ul id="9cece26f-ef5c-4688-bb8b-713472d63391" class="bulleted-list"><li style="list-style-type:disc">note the authorization token expires within 1 hour by default</li></ul><h2 id="e8a13e55-0dab-4e9a-b849-78997fd520af" class="">Compute Instances</h2><h3 id="97607a87-1f35-4a78-babb-32c9e59a51a7" class="">General</h3><ul id="785ebdfb-14df-4ac3-a1f3-2cf456196142" class="bulleted-list"><li style="list-style-type:disc">just because an access scope blocks a certain command, does not mean that any variations of that command cannot be run<ul id="c43a6b25-bec7-43af-8ff8-e3a13e312156" class="bulleted-list"><li style="list-style-type:circle">e.g. if <code>gsutil ls</code> returns no storage buckets, you may still be able to query a storage bucket by specifying the name of the bucket for example <code>gsutil ls gs://storage_bucket_example-1234567</code></li></ul></li></ul><p id="4731cf4e-5448-451f-b4d6-7e6bbdc8b618" class="">Enumerate scripts within the following areas:<div class="indented"><ol type="1" id="d20637f7-9dae-4d26-8cf4-067b8b6fa171" class="numbered-list" start="1"><li>Instance metadata</li></ol><ol type="1" id="8567f381-ac15-4aba-aa01-367bf974bd78" class="numbered-list" start="2"><li>Local filesystem</li></ol><ol type="1" id="2ed79c34-1eff-4208-b356-fee17f3d261c" class="numbered-list" start="3"><li>Service unit files</li></ol><ol type="1" id="6ea3479a-fe11-48c0-95e1-0aaa31120420" class="numbered-list" start="4"><li>etc.</li></ol></div></p><ul id="cb1d4a1f-0962-4f80-a2d5-e1c8c93e5069" class="bulleted-list"><li style="list-style-type:disc">scripts help tell what the instance is meant for and what it has access to </li></ul><h3 id="05544736-8d54-4d56-bbaf-c6e38ecabe2a" class="">Modifying Instance Metadata</h3><p id="8ab39942-f1ce-4e6b-83d7-207f1cefa19d" class=""><strong>Default Service Account</strong></p><p id="530d236f-4c14-48ea-91ca-ddf39392785d" class="">The following access scopes are offered for default service accounts:<div class="indented"><ol type="1" id="58d9219b-e861-4437-a00f-2dc20eeaacb4" class="numbered-list" start="1"><li>Allow default access (default)</li></ol><ol type="1" id="f08e23b4-d817-43d1-9010-89de2688d379" class="numbered-list" start="2"><li>Allow full access to all Cloud APIs</li></ol><ol type="1" id="5fd12bff-2d6c-435e-a495-6fd85e8709e2" class="numbered-list" start="3"><li>Set access for each API </li></ol></div></p><ul id="7b0086f9-9b8b-4364-8781-e3b66bd13d3d" class="bulleted-list"><li style="list-style-type:disc">if 3 (with compute API access) or 2 is enabled, privesc is potentially possible</li></ul><p id="3337cb39-9f74-49a7-8b90-adbcb5ffd9c2" class="">
</p><p id="bc16c200-cd6f-493f-8a81-1138205d7888" class=""><strong>Custom Service Account</strong></p><ul id="482461bf-4273-47eb-a7be-59bb1cb81465" class="bulleted-list"><li style="list-style-type:disc">Google discourages using access scopes for custom service accounts</li></ul><p id="275ff16a-ff43-488a-a846-a97a39438a2c" class="">One of the following privileges necessary for privesc:<div class="indented"><ol type="1" id="8971f0d1-d355-4a4b-93ba-d2fc87e55938" class="numbered-list" start="1"><li><code>compute.instances.setMetadata</code> </li></ol><ol type="1" id="7e1c7597-67f6-49b7-ab70-5f39b96c20ee" class="numbered-list" start="2"><li><code>compute.projects.setCommonInstanceMetadata</code></li></ol></div></p><p id="49b2411e-ad55-43f5-aa4c-ef99d8182a00" class="">It is necessary to be able to authenticate to either <a href="https://www.googleapis.com/auth/compute"><code>https://www.googleapis.com/auth/compute</code></a> or <code>https://www.googleapis.com/auth/cloud-platform</code></p><p id="bf7a3cfe-69e6-4547-abb8-3a6e82a36245" class="">
</p><p id="61f37175-b1ba-4612-babe-30e802fff3af" class=""><span style="border-bottom:0.05em solid">Adding SSH Key to Metadata</span></p><ul id="319f4efa-d17d-45d1-83f3-68a7f2d19c59" class="bulleted-list"><li style="list-style-type:disc">Linux GCP systems typically run Python Linux Guest Environment within Compute Engine scripts<ul id="f227a23e-bd7c-4695-8df9-7c0a14874853" class="bulleted-list"><li style="list-style-type:circle">account daemon queries metadata for changes to authorized SSH keys, and will add a new key to an existing user or a user with <code>sudo</code> rights </li></ul><ul id="a819af8c-8102-4a28-8084-4118aa7ccc42" class="bulleted-list"><li style="list-style-type:circle">if custom project metadata can be modified, persistence is established on all systems within the GCP project running the accounts daemon<code>Block project-wide SSH keys</code> option enabled</li></ul></li></ul><p id="0b0cec0f-c485-423e-b7e1-8d76f5d3afdc" class=""><span style="border-bottom:0.05em solid">Adding SSH Key to Existing Privileged User</span></p><p id="fc8094a1-822f-4847-8be6-0a7b7dd3f80c" class=""><code>gcluod compute instance describe &lt;INSTANCE&gt; --zone &lt;ZONE&gt;</code></p><p id="044344ca-80dd-4978-8b97-75abac2d865b" class="">This returns something like the following:</p><pre id="a5433159-5957-4860-a50e-2580cebe342a" class="code code-wrap"><code>[...]
- key: ssh-keys
     value: |-
       high-priv-user:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC/SQup1eHdeP1qWQedaL64vc7j7hUUtMMvNALmiPfdVTAOIStPmBKx1eN5ozSySm5wFFsMNGXPp2ddlFQB5pYKYQHPwqRJp1CTPpwti+uPA6ZHcz3gJmyGsYNloT61DNdAuZybkpPlpHH0iMaurjhPk0wMQAMJUbWxhZ6TTTrxyDmS5BnO4AgrL2aK+peoZIwq5PLMmikRUyJSv0/cTX93PlQ4H+MtDHIvl9X2Al9JDXQ/Qhm+faui0AnS8usl2VcwLOw7aQRRUgyqbthg+jFAcjOtiuhaHJO9G1Jw8Cp0iy/NE8wT0/tj9smE1oTPhdI+TXMJdcwysgavMCE8FGzZ high-priv-user
       low-priv-user:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC2fNZlw22d3mIAcfRV24bmIrOUn8l9qgOGj1LQgOTBPLAVMDAbjrM/98SIa1NainYfPSK4oh/06s7xi5B8IzECrwqfwqX0Z3VbW9oQbnlaBz6AYwgGHE3Fdrbk[...]</code></pre><ol type="1" id="499a512b-f5a7-4884-aa86-6f53b6f4bd29" class="numbered-list" start="1"><li>Create a key for <code>high-priv-user</code><ol type="a" id="a64dad49-776e-4679-a74d-1e669ce1d1eb" class="numbered-list" start="1"><li><code>ssh-keygen -t rsa -C &quot;high-priv-user&quot; -f ./key -P &quot;&quot;</code></li></ol></li></ol><ol type="1" id="3475420f-d440-4b85-a102-4c9bc8453a75" class="numbered-list" start="2"><li>Edit the public key so that it matches the format of the <code>high-priv-user</code> public key file</li></ol><ol type="1" id="e0dbbe88-7d7f-4cb7-a203-bda66fbcdbb9" class="numbered-list" start="3"><li>Add the new key to the instance metadata<ol type="a" id="67819f8e-e077-4e82-9cb0-dcebd72d37b2" class="numbered-list" start="1"><li><code>gcloud compute instances add-metadata &lt;INSTANCE&gt; --metadata-from-file ssh-keys=ssh_public_file.txt</code></li></ol></li></ol><p id="c59441bd-08d5-4b20-bd22-287491f33cc5" class="">
</p><p id="f3769b62-e1b5-4618-8be7-947fd1a403da" class=""><span style="border-bottom:0.05em solid">Creating New User with SSH Key</span></p><ul id="24c196b6-977a-4d48-a80f-c0ce515db447" class="bulleted-list"><li style="list-style-type:disc">the same process can be used (1-3), however a new username should be specified</li></ul><ul id="b89bee70-5fc7-40a3-b7e5-1e03bfa15874" class="bulleted-list"><li style="list-style-type:disc">this gives the new user <code>sudo</code> permissions</li></ul><p id="f6000e80-be54-4e09-939e-9c8ba7d29186" class="">
</p><p id="42a3dcb1-b136-46e1-a4b0-d3a61f80082f" class=""><span style="border-bottom:0.05em solid">Sudo to Existing Session</span></p><p id="b68ede84-854c-42d1-8b2c-f422625fba28" class="">Use the following command to generate a new SSH key, add your current username to <code>google-sudoers</code> group, and initiate an SSH session:</p><p id="ad640a4f-7083-490e-b7a5-fa3820f53b98" class=""><code>gcloud compute ssh &lt;INSTANCE_NAME&gt;</code><div class="indented"><ul id="e0b1e95e-6ad5-40aa-afcf-02a7fb083ea0" class="bulleted-list"><li style="list-style-type:disc">note this may cause more changes to the target instance’s metadata than the manual step-by-step process described above</li></ul><ul id="37c75f01-b0b1-4d9f-9738-6480254a666e" class="bulleted-list"><li style="list-style-type:disc">this uses your current username</li></ul></div></p><p id="cad574a9-1e6e-429d-a117-ee700b13ee56" class="">
</p><p id="b2371b4a-e408-4320-8fb0-d63dc4b1f86c" class=""><strong>  </strong><span style="border-bottom:0.05em solid">OS Login</span></p><ul id="91630b10-10aa-4736-bd08-3e28509dffe1" class="bulleted-list"><li style="list-style-type:disc">links Google user or service account to Linux identity</li></ul><ul id="038939b6-95dd-40d9-9c45-2ea189db9916" class="bulleted-list"><li style="list-style-type:disc">IAM permissions dictate the authorization of this request</li></ul><ul id="6fb1d6ba-4f83-4511-b2e0-3c40f8f0961d" class="bulleted-list"><li style="list-style-type:disc">enabled at project or instance level with the metadata key of <code>enable-oslogin = TRUE</code></li></ul><ul id="48d332c4-9fad-49f7-9518-2013d6ec3a12" class="bulleted-list"><li style="list-style-type:disc">2FA OS login enabled with <code>enable-oslogin-2fa = TRUE</code></li></ul><ul id="e18ecddf-d31b-4e45-a6dd-4eeda38ac1c3" class="bulleted-list"><li style="list-style-type:disc"><code>roles/compute.osLogin</code> and <code>roles/compute.osAdminLogin</code> control SSH access to instances with enabled OS Login<ul id="5f8463d6-3fe3-469f-a278-976d489fe267" class="bulleted-list"><li style="list-style-type:circle">note the former is without sudo access while the latter is with sudo access</li></ul></li></ul><p id="66d0a9e4-97a8-4f8d-a66c-92eb31b92b00" class="">
</p><ul id="d054344c-eedf-4f2a-b470-6327dc1bd0c4" class="bulleted-list"><li style="list-style-type:disc">by adding one’s SSH key to the project metadata, access to all instances can be achieved as long as the instance does not have the <code>Block pojrect-wide SSH keys</code> option enabled:<p id="72c252ae-7fb2-440e-8cab-da2fdfb81840" class=""><code>gcloud compute project-info add-metadata --metadata-from-file ssh-keys=my_public_ssh-key.txt</code></p></li></ul><h3 id="4bd8db62-b0a5-441f-b5f4-9c74ddcf154d" class="">Bypassing Access Scopes</h3><p id="7fe6c184-692a-4c9a-ace9-f40e443b05dd" class=""><mark class="highlight-orange">Access scopes are not a security mechanism (stated by Google themselves)</mark></p><p id="7a04088e-cec9-4445-beae-fabc9c534c4f" class=""><strong>Find Token Access Scopes</strong></p><pre id="d1cd01c8-73d4-4f88-8573-204ddc663c3e" class="code code-wrap"><code>TOKEN=&#x27;gcloud auth print-access-token&#x27;
curl https://www.googleapis.com/oauth2/v1/tokeninfo?access_token=$TOKEN</code></pre><ul id="51ebe020-191a-47df-8902-c79616d1a7e4" class="bulleted-list"><li style="list-style-type:disc">access scopes have “no effect when making requests not authenticated through OAuth”<ul id="2a29f664-05c5-4ec7-b7cb-6ba7fa3a1b4b" class="bulleted-list"><li style="list-style-type:circle">search for an RSA private key to authenticate to the Google Cloud API and request a new OAuth token<p id="1e049cbd-e9db-4a00-9f5d-f0909f1679e3" class=""><code>gcloud auth activate-service-account --key-file &lt;FILE&gt;</code></p></li></ul></li></ul><p id="89286ed4-0346-4917-ace9-f8f2cb2d6877" class="">
</p><p id="6420ac3e-ad49-4998-9d92-3c21e77c8e9b" class=""><strong>Check for Service Accounts with Exported Key Files</strong></p><pre id="43b0f2c1-b717-4023-83ed-2477716ae5d8" class="code"><code>for i in $(gcloud iam service-accounts list --format=&quot;table[no-heading](email)&quot;); do
    echo Looking for keys for $i:
    gcloud iam service-accounts keys list --iam-account $i
done</code></pre><ul id="159bcf7c-9661-45b1-8cb9-b95baa4591ec" class="bulleted-list"><li style="list-style-type:disc">default name for service account key file is <code>&lt;PROJECT_ID&gt;-&lt;PORTION_OF_KEY_ID&gt;.json</code></li></ul><p id="6f50639f-3125-442e-8c62-b1bfac91e869" class="">
</p><ul id="3b6861e1-4d52-4270-90e6-213acc6b4576" class="bulleted-list"><li style="list-style-type:disc">if access scopes are too restrictive, check if there is another instance that is more permissive<ul id="f42364f7-c764-4cff-bbc0-547ec7230dd2" class="bulleted-list"><li style="list-style-type:circle"><code>gcloud compute instances list --quiet</code></li></ul></li></ul><ul id="4e38aeb8-0372-44e9-b2ff-415ef365b3e2" class="bulleted-list"><li style="list-style-type:disc">check if an instance has the default service account (<code>PROJECT_NUMBER-compute@developer.gserviceaccount.com</code>)</li></ul><p id="799ba4af-b9cf-409a-978a-67ac0e1fd374" class="">
</p><h3 id="79c32afa-c47f-4da8-b145-aae2a586e1fd" class="">Steal GCloud Authorizations</h3><ul id="73792dc4-e4e5-4284-be7c-d71fc750479c" class="bulleted-list"><li style="list-style-type:disc">look for the following files:</li></ul><pre id="b486bb3e-6b44-4815-9542-d8b7c7c32452" class="code"><code>~/.config/gcloud/credentials.db
~/.config/gcloud/legacy_credentials/[ACCOUNT]/adc.json
~/.config/gcloud/legacy_credentials/[ACCOUNT]/.boto
~/.credentials.json</code></pre><h2 id="4e0f8397-fc1f-4b6e-b0ac-32f8ee4d74d0" class="">Service Account Impersonation</h2><p id="f6858ad6-e5e0-404a-906f-f78b5150371f" class="">Three ways to impersonate a service account:</p><ol type="1" id="6acde08c-7b15-47e8-bdec-75ee26e20ce3" class="numbered-list" start="1"><li>Authentication using RSA private keys </li></ol><ol type="1" id="9a3629ca-a7aa-4075-a074-b1f40f2ef3f6" class="numbered-list" start="2"><li>Authorization using Cloud IAM policies </li></ol><ol type="1" id="58a53c91-b4c3-4642-bfec-e1f3bc827d34" class="numbered-list" start="3"><li>Deploying jobs on GCP services </li></ol><p id="c14cd801-2e45-4917-8e41-f73402ba34ca" class="">
</p><ul id="e9242844-56e4-48e2-b133-11232801c856" class="bulleted-list"><li style="list-style-type:disc">can potentially impersonate another account with the <code>iam.serviceAccountTokenCreator</code> permission</li></ul><ul id="74111d03-bd9c-455f-b302-ffdc3327a716" class="bulleted-list"><li style="list-style-type:disc">if you have <code>Owner</code> access, you can try logging into the web interface<ul id="b7837c49-b5ae-43d0-9f94-eedf6e4ac649" class="bulleted-list"><li style="list-style-type:circle">service accounts can’t access web interface, but you can provide <code>Editor</code>access to any arbitrary <code>@gmail.com</code> account and then login (can’t provide <code>Owner</code> access)</li></ul><pre id="d0b434b0-b010-4970-8a19-afca412d2311" class="code code-wrap"><code>gcloud projects add-iam-policy-binding &lt;PROJECT&gt; --member user:0xd4y@gmail.com --role roles/editor</code></pre></li></ul><ul id="dc776064-8d89-4c3f-81d5-57af96bfea3f" class="bulleted-list"><li style="list-style-type:disc">you can use <code>--impersonate-service-account</code> flag to execute a command using the specified service account:<ul id="20c48310-299b-400e-b505-6922708104a3" class="bulleted-list"><li style="list-style-type:circle">For example: <code>gcloud compute instances list --impersonate-service-account &lt;SERVICE_ACCOUNT&gt;</code></li></ul></li></ul><h2 id="7aecfdcf-4ac6-4fce-a482-fe8b6c41475a" class="">Accessing Databases</h2><ul id="2f88bac3-0a59-4483-9775-10d7aee3b99d" class="bulleted-list"><li style="list-style-type:disc">check database backups in storage buckets, and of course check other juicy information within instances</li></ul><ul id="58901fe2-967e-41e4-8fef-1cae4952d6cd" class="bulleted-list"><li style="list-style-type:disc"> some <code>gcloud</code> commands are made specifically for exporting data<ul id="77272fd9-88da-4a7d-900d-b932940a2935" class="bulleted-list"><li style="list-style-type:circle">need to write database to storage bucket first before downloading it</li></ul></li></ul><p id="c67fa521-a402-4f13-ad08-4af76141796e" class=""><strong>Finding databases across project</strong></p><pre id="309d75fe-2150-4bc1-b89c-36f10b73b141" class="code code-wrap"><code>Cloud SQL
=============
gcloud sql instances list
gcloud sql databases list --instance [INSTANCE]

Cloud Spanner
==============
gcloud spanner instances list
gcloud spanner databases list --instance [INSTANCE]

Cloud Bigtable
==============
gcloud bigtable instances list</code></pre><h2 id="9dfe8ef8-31ff-416a-b740-0a72e8b5b343" class="">Storage Buckets</h2><ul id="2e29a632-13f6-497a-807b-46ed4e8c940d" class="bulleted-list"><li style="list-style-type:disc">note that default instance permissions allow read access to storage buckets</li></ul><ul id="6afa8355-f154-4948-9d8f-e4b3de6594ba" class="bulleted-list"><li style="list-style-type:disc">can be found with wordlists, source code, etc.</li></ul><ul id="bef27fad-b30f-415b-8534-48630e714206" class="bulleted-list"><li style="list-style-type:disc">use <code>gsutil</code> to interact with storage buckets</li></ul><ul id="737a513a-c684-480b-a1ca-29cee146684f" class="bulleted-list"><li style="list-style-type:disc">if <code>gsutil ls</code> returns access denied, access to storage buckets is still potentially possible, but requires the bucket name to be specified</li></ul><p id="76e1e4aa-6866-4b7c-a68b-3f0bd7e29c10" class="">
</p><p id="ae39d85a-3bab-4390-ae9f-2bf021a6c806" class=""><strong>Bash Oneliner for Bruteforcing Bucket Names</strong></p><p id="cbe0cf21-8778-45a1-b628-9dc7017449dc" class=""><code>for i in $(cat wordlist.txt); do gsutil ls -r gs://&quot;$i&quot;; done</code></p><h2 id="956083e1-02f0-4fb5-89eb-b5776b45fae0" class="">Decrypting Secrets</h2><ul id="ae71ff55-7e43-4a80-b018-b681594c5959" class="bulleted-list"><li style="list-style-type:disc">cryptographic keys stored within Cloud KMS (Key Management Service)</li></ul><ul id="4c4b51ef-2412-4e7b-9d50-dda2aa694228" class="bulleted-list"><li style="list-style-type:disc">individual keys stored in key rings</li></ul><h3 id="1dae6180-a121-4a1c-85c5-c45a0ab2c909" class="">Enumeration</h3><ul id="649eec5a-8a04-4ee4-9637-d3c396f6d524" class="bulleted-list"><li style="list-style-type:disc">without GCloud enumeration permissions, try searching for keys in documentation, scripts, and bash history</li></ul><table id="4c9d29d5-c2fa-41d1-8a2b-55d1a6d4a39d" class="simple-table"><tbody><tr id="0196a64b-70ff-4386-a846-59921c5358bd"><td id="rU`e" class="" style="width:486px"><strong>Command</strong></td><td id="&lt;}V{" class="" style="width:218px"><strong>Description</strong></td></tr><tr id="e2c856fa-431e-40d5-a14f-9c71e54d64c3"><td id="rU`e" class="" style="width:486px"><code>gcloud kms keyrings list --location global</code></td><td id="&lt;}V{" class="" style="width:218px">Lists global keyrings available</td></tr><tr id="9be533e4-e9ea-46f4-a18c-0d4ce4dbdf19"><td id="rU`e" class="" style="width:486px"><code>gcloud kms keys list --keyring &lt;KEYRING_NAME&gt; --location global</code></td><td id="&lt;}V{" class="" style="width:218px">Lists keys inside a keyring</td></tr><tr id="c9e67dc3-49b6-40c5-975e-4aa0271bd057"><td id="rU`e" class="" style="width:486px"><code>gcloud kms decrypt --ciphertext-file=&lt;INFILE&gt; --plaintext-file=&lt;OUTFILE&gt; --key &lt;KEY&gt; --keyring &lt;KEYRING&gt; --location global</code></td><td id="&lt;}V{" class="" style="width:218px">Decrypts file using a key</td></tr></tbody></table><h2 id="7b4dfdcd-35b8-4be4-98b8-cde58a376286" class="">Serial Console Logs</h2><ul id="7710ac75-4e5a-4770-bd3e-53226d8646cb" class="bulleted-list"><li style="list-style-type:disc">output from compute instances written from OS and BIOS to serial ports</li></ul><p id="32644c3f-9bee-4857-bf0e-8557b74daea2" class="">Two ways to view the log files from the serial ports:</p><ol type="1" id="45a71754-5ed1-43d2-b9a2-a9f457de6b52" class="numbered-list" start="1"><li>Via Compute API<ul id="7dabfc40-373d-4f3f-aac3-422ab78b0fdf" class="bulleted-list"><li style="list-style-type:disc">can be executed even with the <code>Compute: Read Only</code> access scope restriction</li></ul><ul id="0fe25600-c82d-48af-9039-18617b64437a" class="bulleted-list"><li style="list-style-type:disc"><code>gcloud compute instances get-serial-port-output &lt;INSTANCE_NAME&gt; --port &lt;PORT&gt; --start start --zone &lt;ZONE&gt;</code> </li></ul></li></ol><ol type="1" id="9a2b0f1c-d564-4122-a287-7bfe359ccc3d" class="numbered-list" start="2"><li>Via Cloud Logging<ul id="b51f6632-f5ee-402d-9165-9db3f831775b" class="bulleted-list"><li style="list-style-type:disc">serial logs stored in Cloud Logging if enabled by admin</li></ul><ul id="a84407dc-22a5-4781-ab89-d2ee9d736d7b" class="bulleted-list"><li style="list-style-type:disc">can be accessed with logging read permissions</li></ul></li></ol><h2 id="e0452e20-dd37-48b1-8bf9-1780b2736d85" class="">Custom Images</h2><ul id="d2e98300-70cf-4485-9cf9-5f1aa86b5fce" class="bulleted-list"><li style="list-style-type:disc">some images may contain sensitive information which you can exfiltrate and use for a new VM</li></ul><p id="5790d12b-a5ca-4b16-b2de-1b357893a50d" class=""><strong>Find List of Custom Images</strong></p><p id="6592a498-bcf7-4c81-aeaa-1aa562df22e3" class=""><code>gcloud compute images list --no-standard-images</code></p><p id="12676c1b-cdba-493d-99a7-96505f677671" class=""><strong>Export Images</strong></p><p id="07643f11-a47a-4523-b3f9-86654bea5525" class=""><code>gcloud compute images export --image &lt;IMAGE_NAME&gt; --export-format qcow2 --destination-uri &lt;BUCKET&gt;</code></p><h2 id="4bf0e21b-a818-4a12-bf3a-7df8bf72e416" class="">Custom Templates</h2><ul id="42e26f9e-9f18-421b-94a0-e8719c717916" class="bulleted-list"><li style="list-style-type:disc">instance templates allow deployment of VMs with specific configurations<ul id="e1583f7a-674c-4aa0-b0bc-b742ac24f34c" class="bulleted-list"><li style="list-style-type:circle">these configurations can tell the VM which image to use, startup script, labels, etc.</li></ul></li></ul><table id="01e2f2c3-7eff-4433-8d39-fe11b5a579ac" class="simple-table"><tbody><tr id="0227b35b-c702-4ae5-a503-260215b5973a"><td id="vzy|" class="" style="width:280px"><strong>Command</strong></td><td id="CrAN" class="" style="width:367px"><strong>Description</strong></td></tr><tr id="1e58bca4-904f-4e5c-b16c-d30b85412a75"><td id="vzy|" class="" style="width:280px"><code>gcloud compute instance-templates list</code></td><td id="CrAN" class="" style="width:367px">Lists available templates</td></tr><tr id="37c17496-44d2-44e5-9220-cd3acb62fed2"><td id="vzy|" class="" style="width:280px"><code>gcloud compute instance-templates describe &lt;TEMPLATE_NAME&gt;</code></td><td id="CrAN" class="" style="width:367px">Get details of specific template</td></tr></tbody></table><ul id="b6627fa6-9fa5-46d7-924f-519da935d8a1" class="bulleted-list"><li style="list-style-type:disc">a template can include sensitive data that can be discovered via the instance metadata</li></ul><h2 id="fea88e71-c5e6-4e2a-a2a7-59d282598fa0" class="">StackDriver Logging</h2><ul id="774f3db2-74f8-4210-a445-7e021fb6a29b" class="bulleted-list"><li style="list-style-type:disc">StackDriver is a Google monitoring and logging service<ul id="9586fa6b-4b89-49da-b8d2-c25138660e26" class="bulleted-list"><li style="list-style-type:circle">Google’s equivalent of AWS CloudWatch and CloudTrail</li></ul></li></ul><ul id="64d0fdeb-f6ea-4a02-998b-76ae397f6340" class="bulleted-list"><li style="list-style-type:disc">compute instances require <code>write</code> access to write to log files, however if <code>read</code> permissions are also granted, then logs can be read</li></ul><table id="4a559874-6fd5-48a1-9dab-6773a14d23f1" class="simple-table"><tbody><tr id="b1d98742-0053-486e-b29d-6344125470aa"><td id="h[JN" class="" style="width:269px"><strong>Command</strong></td><td id="`gZ~" class="" style="width:373.00000762939453px"><strong>Description</strong></td></tr><tr id="ecaf1290-3d82-4c5f-9853-8e3ccf5331fc"><td id="h[JN" class="" style="width:269px"><code>gcloud logging logs list</code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Lists log folders in current project </td></tr><tr id="34848f0a-ec5f-4ca2-901d-9e57d88023c4"><td id="h[JN" class="" style="width:269px"><code>gcloud logging read &lt;LOG_FOLDER&gt;</code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Read contents of specific log folder</td></tr><tr id="9d2c9198-d0ba-4de0-936f-35b69137ba8a"><td id="h[JN" class="" style="width:269px"><code>gcloud logging write &lt;LOG_FOLDER&gt; &lt;MESSAGE&gt;</code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Write arbitrary data to a specific log folder. Can be used for distraction.</td></tr></tbody></table><h2 id="1be71d6b-5fcb-4d02-a107-7255fc0623cc" class="">Serverless Services</h2><h3 id="95da81fb-cb97-4bd4-bd91-5824aa948fa1" class="">Cloud Functions</h3><ul id="1cc958ae-b408-40d2-9a1f-13a350bf3f4e" class="bulleted-list"><li style="list-style-type:disc">AWS Lambda equivalent</li></ul><ul id="02735c7a-8733-4734-bb03-6d3ea00a8588" class="bulleted-list"><li style="list-style-type:disc">environment variables can contain secrets just like in AWS</li></ul><table id="03a89c4e-c36c-45f8-9d40-bce873894852" class="simple-table"><tbody><tr id="c1727b32-8f6d-4833-a0a6-f5bf4f823f82"><td id="h[JN" class="" style="width:269px"><strong>Command</strong></td><td id="`gZ~" class="" style="width:373.00000762939453px"><strong>Description</strong></td></tr><tr id="e84ab566-d415-408d-83ad-6db79d3f9bd6"><td id="h[JN" class="" style="width:269px"><code>gcloud functions list</code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Lists available cloud functions </td></tr><tr id="ef24acc8-879b-4986-81c3-ee605e28abb4"><td id="h[JN" class="" style="width:269px"><code>gcloud functions describe &lt;FUNCTION_NAME&gt; </code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Display function configuration and defined environment variables</td></tr><tr id="a35c27b1-2c57-4cb8-8fb5-f071170c0d99"><td id="h[JN" class="" style="width:269px"><code>gcloud functions logs read &lt;FUNCTION_NAME&gt;</code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Get logs of the function executions</td></tr></tbody></table><h3 id="1a337c12-6474-4c97-8b7d-9710806866ab" class="">App Engine</h3><ul id="670f84d1-1dc4-4921-a00e-324e5dfe16e8" class="bulleted-list"><li style="list-style-type:disc">Google App Engine is a serverless cloud computing platform focusing on scalability</li></ul><ul id="d24fe2da-c12a-4f11-84b6-2c558cd393d0" class="bulleted-list"><li style="list-style-type:disc">secrets can be stored in environment variables<table id="a9466679-fb76-48f5-b231-b109874115d0" class="simple-table"><tbody><tr id="7104f558-1368-4c77-bd1e-07facab62eef"><td id="h[JN" class="" style="width:269px"><strong>Command</strong></td><td id="`gZ~" class="" style="width:373.00000762939453px"><strong>Description</strong></td></tr><tr id="dba25120-3f9d-4573-a349-3b40724c087b"><td id="h[JN" class="" style="width:269px"><code>gcloud app versions list</code> </td><td id="`gZ~" class="" style="width:373.00000762939453px">Lists existing versions for all services in the App Engine server </td></tr><tr id="aed93c4f-6038-4211-acac-18b5cf48d928"><td id="h[JN" class="" style="width:269px"><code>gcloud app describe &lt;APP&gt; </code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Displays information about a specific app</td></tr></tbody></table></li></ul><h3 id="bd9ba57d-bbda-45b4-8309-6a7cebd4790c" class="">Cloud Run</h3><ul id="a1e8e126-5347-4cf5-9961-a67a9f75ad42" class="bulleted-list"><li style="list-style-type:disc">check environment variables for secrets</li></ul><ul id="3885834b-09f1-4d38-9391-71eec1a607e6" class="bulleted-list"><li style="list-style-type:disc">opens web server on port 8080 and waits for HTTP GET request<ul id="a3cc4ada-0cde-4313-9fcf-8ba68344b32b" class="bulleted-list"><li style="list-style-type:circle">upon receiving such a request, a job is executed which is logged and outputted via an HTTP response</li></ul></li></ul><ul id="9630cac7-c52c-4413-bc5b-9e2ab6e0ee34" class="bulleted-list"><li style="list-style-type:disc">jobs run in Kubernetes clusters either fully managed by Google or partially managed through <a href="https://cloud.google.com/anthos">Anthos</a><ul id="36c8a2de-6454-475f-b7f9-6adf323061ff" class="bulleted-list"><li style="list-style-type:circle">can be configured with IAM permissions to control which identities can start the job</li></ul><ul id="57148b39-1e98-4eaf-bef2-b851564d5426" class="bulleted-list"><li style="list-style-type:circle">can be configured to be unauthenticated, allowing anyone with the URL to trigger the job and view the log output</li></ul></li></ul><ul id="b51b2e95-756d-412f-88cf-a097201208c4" class="bulleted-list"><li style="list-style-type:disc"><mark class="highlight-orange">be careful about what those jobs do because it could affect production!</mark></li></ul><table id="58f603b3-c046-41cc-939f-f9e7c37a2e5c" class="simple-table"><tbody><tr id="1fad394a-1ecf-445d-9dd8-8a7436474e62"><td id="h[JN" class="" style="width:269px"><strong>Command</strong></td><td id="`gZ~" class="" style="width:373.00000762939453px"><strong>Description</strong></td></tr><tr id="b87440a2-58de-498a-87d9-05e97bbb8dee"><td id="h[JN" class="" style="width:269px"><code>gcloud run services list --platform=managed --format=json</code>  
<code>gcloud run services list --platform=gke --format=json</code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Lists services across available platforms</td></tr><tr id="7268afc2-56ba-49d1-9205-b168efae16c4"><td id="h[JN" class="" style="width:269px">1. <code>curl &lt;URL&gt;
</code>2. <code>curl -H &quot;Authorization: Bearer $(gcloud auth print-identity-token)&quot; &lt;URL&gt;</code></td><td id="`gZ~" class="" style="width:373.00000762939453px">1. Attempt to trigger a job as an unauthenticated user
2. Trigger a job as authenticated user</td></tr></tbody></table><h3 id="8f7cbd77-79f6-4a50-978a-f4bbd8fc90e9" class="">AI Platform</h3><ul id="77a7c673-2c70-45bb-926f-7e8861e1befc" class="bulleted-list"><li style="list-style-type:disc">look for models and jobs</li></ul><table id="7a2681e2-9a74-4477-82c5-28f50c8381e6" class="simple-table"><tbody><tr id="d06ec282-e7cd-40be-a976-dc32af3c6dce"><td id="h[JN" class="" style="width:269px"><strong>Command</strong></td><td id="`gZ~" class="" style="width:373.00000762939453px"><strong>Description</strong></td></tr><tr id="b29741a8-5658-4af4-aca1-9d7159d1d209"><td id="h[JN" class="" style="width:269px"><code>gcloud ai-platforms models list --format=json</code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Lists models</td></tr><tr id="e0481ebe-cde2-4db5-8a99-921fa554e298"><td id="h[JN" class="" style="width:269px"><code>gcloud ai-platform jobs list --format=json</code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Lists jobs</td></tr></tbody></table><h2 id="39e69c66-6575-480e-883b-a8d79dfac461" class="">Cloud Pub/Sub</h2><ul id="2e3c901c-b09a-4f79-a790-459819962c72" class="bulleted-list"><li style="list-style-type:disc">service allowing applications to send messages between each other</li></ul><p id="b335cdb4-dcb2-48c5-a434-da0eb8941f04" class="">Pub/Sub is made up of the following:</p><ol type="1" id="289881c2-0c6e-4204-9775-013ba5bde1f0" class="numbered-list" start="1"><li>Topic - logical group of messages</li></ol><ol type="1" id="977fb9e3-4403-47c8-8382-c8be478408d8" class="numbered-list" start="2"><li>Subscriptions - Allows applications to receive a stream of messages related to a topic, which can be enabled via push notifications (for some Google services), or pull requests (for custom services) </li></ol><ol type="1" id="655e0c2d-44d8-4a09-978f-ed44678ad78b" class="numbered-list" start="3"><li>Messages - data (optionally metadata as well)</li></ol><table id="43418f23-434e-4050-a816-c690ba4bf1f4" class="simple-table"><tbody><tr id="454d81ca-5e47-4a18-9144-dc0291d5d707"><td id="h[JN" class="" style="width:269px"><strong>Command</strong></td><td id="`gZ~" class="" style="width:373.00000762939453px"><strong>Description</strong></td></tr><tr id="93b861f3-6f2b-47c9-ad88-4f5d09b87ccb"><td id="h[JN" class="" style="width:269px"><code>gcloud pubsub topics list</code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Lists topics in project</td></tr><tr id="78eb4d19-bd6a-4fa8-889f-c7249730b511"><td id="h[JN" class="" style="width:269px"><code>gcloud pubsub subscrpitions list --format=json</code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Lists subscriptions for all topics</td></tr><tr id="7691d780-ba6d-4fde-92ad-b5c3b2422c39"><td id="h[JN" class="" style="width:269px"><code>gcloud pubsub subscriptions pull &lt;SUBSCRIPTION_NAME&gt;</code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Pulls one or more messages from a subscriptions</td></tr></tbody></table><ul id="ea39067a-7031-46f3-bd5a-59fc8925120a" class="bulleted-list"><li style="list-style-type:disc">modification of messages can change behavior of application depending on how the application interacts with the messages </li></ul><ul id="0f3bce66-6fbb-420d-b890-c551a4e25a4c" class="bulleted-list"><li style="list-style-type:disc">the pull command could be used to mimic valid applications<ul id="168402af-daaf-4d42-b7ed-8d2137e36f2e" class="bulleted-list"><li style="list-style-type:circle">some messages can be requested that have not yet been delivered</li></ul><ul id="f2408700-de97-4f02-b5af-e9d6534b074d" class="bulleted-list"><li style="list-style-type:circle">this command should not send an ACK back and should not impact other apps </li></ul></li></ul><ul id="0156cdcc-e02d-4bf9-b41b-b68d273206be" class="bulleted-list"><li style="list-style-type:disc">an attacker can ACK a message before it is received by the app to avoid some detection </li></ul><ul id="eacb133b-fea8-44ea-bca0-074b6bafc348" class="bulleted-list"><li style="list-style-type:disc">asking for large sets of data could impact applications (be careful!)</li></ul><h2 id="cc055ada-5549-4a52-9bd4-530cb28ef0ca" class="">Cloud Source Repos</h2><ul id="d8c97d7b-d16a-4e95-a6f5-c440f87e4058" class="bulleted-list"><li style="list-style-type:disc">designed like Git so</li></ul><ul id="b8659b65-cc7b-418a-b66b-5dca23b1783c" class="bulleted-list"><li style="list-style-type:disc">can contain juicy info</li></ul><table id="d1d442a5-25a8-4079-a976-38ec45b1890b" class="simple-table"><tbody><tr id="71a431da-01da-44ca-8de8-8490c7f99b1c"><td id="h[JN" class="" style="width:269px"><strong>Command</strong></td><td id="`gZ~" class="" style="width:373.00000762939453px"><strong>Description</strong></td></tr><tr id="0cda4eeb-1d1f-420b-af0f-848b48244cee"><td id="h[JN" class="" style="width:269px"><code>gcloud source repos list</code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Enumerate available repos</td></tr><tr id="85f204a1-ff7b-4663-a0ac-fe48fea21fd1"><td id="h[JN" class="" style="width:269px"><code>gcloud source repos clone &lt;REPO_NAME&gt;</code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Clone a repo</td></tr></tbody></table><h2 id="0c2c99c5-9105-49ff-8b38-3b8f36a11a61" class="">Cloud Filestore</h2><ul id="e114e98e-46a8-4f2a-8d98-d195f25be798" class="bulleted-list"><li style="list-style-type:disc">database designed for storing small documents </li></ul><ul id="c8f32316-42c8-4d93-b91a-f8153c7365e5" class="bulleted-list"><li style="list-style-type:disc">like AWS DynamoDB</li></ul><ul id="52eaf487-5908-4c99-b56d-f63ec34ec000" class="bulleted-list"><li style="list-style-type:disc">filestores can be mounted</li></ul><p id="0b82721f-bf38-47b3-a339-deb37c2d829b" class=""><strong>List Filestore Instances</strong> </p><p id="9cb2310b-f070-41b9-a898-4f1676de4ec4" class=""><code>gcloud filestore instances list --format=json</code></p><h2 id="0e2f40d3-0aa7-4d9e-a3a8-2eda4beffb79" class="">Kubernetes</h2><ul id="541ce27e-b4e4-45ea-8834-e6646a3c2fd5" class="bulleted-list"><li style="list-style-type:disc">container service for scaling, management, and software deployment </li></ul><table id="3e130c7f-ac0c-4292-9bca-6ee2faf8222d" class="simple-table"><tbody><tr id="0e07f919-dd3f-41ea-ad56-045ac324ae53"><td id="h[JN" class="" style="width:269px"><strong>Command</strong></td><td id="`gZ~" class="" style="width:373.00000762939453px"><strong>Description</strong></td></tr><tr id="411d49e2-8436-4383-986d-7123b661cdfe"><td id="h[JN" class="" style="width:269px"><code>gcloud container clusters list</code></td><td id="`gZ~" class="" style="width:373.00000762939453px">List container clusters in current project</td></tr><tr id="c00172ed-9b85-44aa-bd4d-2cbfaffe4c2f"><td id="h[JN" class="" style="width:269px"><code>gcloud container clusters get-credentials &lt;CLUSTER_NAME&gt; --region &lt;REGION&gt;</code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Authenticates your <code>~/..kube/config</code> file to include the cluster so that you can use <code>kubectl</code>.</td></tr><tr id="e01f9590-2c88-4c02-a2de-953f0faf25ba"><td id="h[JN" class="" style="width:269px"><code>kubectl cluster-info</code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Get information about the cluster.</td></tr></tbody></table><p id="6906012c-0d31-461b-a7cb-73b0ea9cb4db" class="">Kubectl cheat sheet: <a href="https://kubernetes.io/docs/reference/kubectl/cheatsheet/">https://kubernetes.io/docs/reference/kubectl/cheatsheet/</a></p><h2 id="f63c53d2-e183-4a49-9108-d8ef81f19659" class="">Secrets Management</h2><ul id="bb525f67-fb4f-440b-9ea8-1cc6173fcd89" class="bulleted-list"><li style="list-style-type:disc">stores passwords, API keys, certificates, etc.</li></ul><table id="d377f73d-85f4-4858-bd17-6203dacd1960" class="simple-table"><tbody><tr id="a5c9109d-84cc-40a8-8566-f2b93bfdbbcd"><td id="h[JN" class="" style="width:269px"><strong>Command</strong></td><td id="`gZ~" class="" style="width:373.00000762939453px"><strong>Description</strong></td></tr><tr id="7c7c60e9-cb54-4166-866d-6de246ec26dc"><td id="h[JN" class="" style="width:269px"><code>gcloud secrets list</code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Lists secrets in vault</td></tr><tr id="9a7eb4fc-40bb-4260-a0a4-b9cbdc8b6244"><td id="h[JN" class="" style="width:269px"><code>gcloud secrets describe &lt;SECRET&gt; </code></td><td id="`gZ~" class="" style="width:373.00000762939453px">Get the value of the secret.</td></tr></tbody></table><h3 id="dfd7ccbc-957f-400d-97ae-9a8fce7b3b0b" class="">Local System Secrets</h3><ul id="06f2b9d2-2e8c-43f0-9d4e-119a000d0a09" class="bulleted-list"><li style="list-style-type:disc">with internal access to a system search temporary directories, history files, environment variables, scripts, etc. </li></ul><pre id="29febc3b-1a85-4785-9df9-03ca1c250b5f" class="code code-wrap"><code>TARGET_DIR=&quot;/path/to/whatever&quot;

# Service account keys
grep -Pzr &quot;(?s){[^{}]*?service_account[^{}]*?private_key.*?}&quot; \
    &quot;$TARGET_DIR&quot;

# Legacy GCP creds
grep -Pzr &quot;(?s){[^{}]*?client_id[^{}]*?client_secret.*?}&quot; \
    &quot;$TARGET_DIR&quot;

# Google API keys
grep -Pr &quot;AIza[a-zA-Z0-9\\-_]{35}&quot; \
    &quot;$TARGET_DIR&quot;

# Google OAuth tokens
grep -Pr &quot;ya29\.[a-zA-Z0-9_-]{100,200}&quot; \
    &quot;$TARGET_DIR&quot;

# Generic SSH keys
grep -Pzr &quot;(?s)-----BEGIN[ A-Z]*?PRIVATE KEY[a-zA-Z0-9/\+=\n-]*?END[ A-Z]*?PRIVATE KEY-----&quot; \
    &quot;$TARGET_DIR&quot;

# Signed storage URLs
grep -Pir &quot;storage.googleapis.com.*?Goog-Signature=[a-f0-9]+&quot; \
    &quot;$TARGET_DIR&quot;

# Signed policy documents in HTML
grep -Pzr &#x27;(?s)&lt;form action.*?googleapis.com.*?name=&quot;signature&quot; value=&quot;.*?&quot;&gt;&#x27; \
    &quot;$TARGET_DIR&quot;</code></pre><h1 id="5f5e2e92-c686-47d9-ad62-3c224f6928e1" class="">Networking</h1><h2 id="53132471-6e8e-4578-82de-5beee1c6d0c3" class="">Firewall</h2><ul id="7cfe46d7-4c39-4be5-8512-64e0b0259557" class="bulleted-list"><li style="list-style-type:disc">every project is given a default VPC which contains the following rules for all instances:<ol type="1" id="a81474df-8d77-48c4-ae70-b4855478b016" class="numbered-list" start="1"><li><code>default-allow-internal</code> - allows all traffic from other instances on the same network</li></ol><ol type="1" id="8a3f42ab-0134-4424-9ac6-916a592203cf" class="numbered-list" start="2"><li><code>default-allow-ssh</code> - allows port 22 traffic from everywhere</li></ol><ol type="1" id="575eff21-96b2-4e31-8869-d87f809325c5" class="numbered-list" start="3"><li><code>default-allow-rdp</code> - allows port 3389 traffic from everywhere</li></ol><ol type="1" id="bdf59615-8774-438d-b683-082e756af50c" class="numbered-list" start="4"><li><code>default-allow-icmp</code> - allows ping from everywhere</li></ol></li></ul><h2 id="e710fc28-94e2-474c-bb1d-6ee5b59f4356" class="">Enumeration</h2><p id="a018a506-820f-4251-b2be-ce0dd0be61ce" class="">View all subnets in current project:</p><p id="661986f2-4aea-4f25-8937-e967ce8a6caf" class=""><code>gcloud compute networks subnets list</code></p><p id="8ceaa3aa-f5f8-4cbd-a909-4f675a43869d" class="">View all internal/external IP addresses in project:</p><p id="5eec4493-9faf-44e9-9194-2e4bc173b539" class=""><code>gcloud compute instances list</code></p><p id="7500aaf8-0e8c-4122-bd38-16749bd525d3" class="">View open ports of all instances</p><ul id="681e7b5f-4494-494f-b675-12b010500fe8" class="bulleted-list"><li style="list-style-type:disc"><strong><mark class="highlight-orange">Running nmap from within an instance can trigger an alert</mark></strong><ul id="55fdcc83-41b6-4e2c-9662-9508c1e2c238" class="bulleted-list"><li style="list-style-type:circle">likelihood of trigger increases if scanning public IP addresses outside of current project</li></ul></li></ul><ul id="cc9f278c-fdd4-4190-80b5-d06d78d56fad" class="bulleted-list"><li style="list-style-type:disc">there may be an insecure application that can be exploited to achieve elevated access</li></ul><ul id="9562898f-e8db-490c-8994-1febfddef957" class="bulleted-list"><li style="list-style-type:disc">port enumeration should be interpreted by viewing firewall rules, network tags, service accounts, and instances within a VPC (see <a href="https://gitlab.com/gitlab-com/gl-security/threatmanagement/redteam/redteam-public/gcp_firewall_enum">gcp_firewall_enum</a>)</li></ul><p id="4c7c71e3-ce03-4472-83a0-67c31a0163c1" class="">
</p><h1 id="7472f727-17ef-485b-a091-d66957103b2c" class="">G Suite</h1><ul id="83eb189b-b99c-4743-9274-0986cdb57d9f" class="bulleted-list"><li style="list-style-type:disc">uses completely different API from Google Cloud</li></ul><ul id="61348d02-603b-4cbf-8f47-812575a185d8" class="bulleted-list"><li style="list-style-type:disc">GCP service accounts can access G Suite data using domain-wide delegation<ul id="65a90a76-8095-4eca-bb2d-a5e9e5b64aa7" class="bulleted-list"><li style="list-style-type:circle">can be viewed in the web interface via IAM → Service Accounts</li></ul></li></ul><h2 id="0f328051-bce2-4ad7-b0dc-13a361d03aee" class="">Authenticating to G Suite</h2><ul id="d8fafc32-c58d-4388-8a12-fb8a7411067a" class="bulleted-list"><li style="list-style-type:disc">need exported service accounts credentials in JSON format</li></ul><ul id="b255b4d1-de21-4a75-806f-9dc84134613e" class="bulleted-list"><li style="list-style-type:disc">service accounts cannot authenticate to G Suite, and therefore you need to impersonate valid G Suite users<ul id="8c53b8e6-f330-4c1f-8689-2247a8fbac49" class="bulleted-list"><li style="list-style-type:circle">(see <a href="https://gitlab.com/gitlab-com/gl-security/threatmanagement/redteam/redteam-public/gcp_misc/-/blob/master/gcp_delegation.py">gcp_delegation</a>) </li></ul></li></ul><h1 id="655bc3a8-ee95-443b-9d2c-bd0d814732f2" class="">Tools</h1><p id="c0ed8766-1ff0-4b3c-bce3-07c61f3f9518" class=""><a href="https://gitlab.com/gitlab-com/gl-security/threatmanagement/redteam/redteam-public/gcp_firewall_enum">gcp_firewall_enum</a></p><ul id="2c3bfc13-eb9d-438a-954c-bd8dd3992c28" class="bulleted-list"><li style="list-style-type:disc">port scans for compute instances exposed to the internet</li></ul><p id="de420f06-2b88-4c3b-8e2e-07b0e79a4888" class=""><a href="https://gitlab.com/gitlab-com/gl-security/threatmanagement/redteam/redteam-public/gcp_enum">gcp_enum</a></p><ul id="df7e793d-7beb-4812-b12d-e95e8fb9efe8" class="bulleted-list"><li style="list-style-type:disc">script full of enumeration commands</li></ul><p id="62864b96-7280-47ef-aa75-aad0cc68709f" class=""><a href="https://gitlab.com/gitlab-com/gl-security/threatmanagement/redteam/redteam-public/gcp_misc">gcp_misc</a></p><ul id="8e12f450-1655-4cb8-ae67-226c80d69d57" class="bulleted-list"><li style="list-style-type:disc">a collection of tools for attacking GCP environments</li></ul><ul id="07b33248-8dc8-44a8-a39b-ccd2c00e3687" class="bulleted-list"><li style="list-style-type:disc">contains <a href="https://gitlab.com/gitlab-com/gl-security/threatmanagement/redteam/redteam-public/gcp_misc/-/blob/master/gcp_delegation.py">gcp_delegation</a> for listing user directory and creating a new admin account</li></ul><p id="a8268d52-29ce-44a6-9879-1a3007f5c73d" class="">
</p></div></article></body></html>
