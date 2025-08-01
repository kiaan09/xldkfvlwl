<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>D&D 캐릭터 시트</title>
  <style>
    /* ==================================================
	Dungeons & Dragons 5판 커스텀 시트
	제작:라브 (@rigel_rpg)
	버전:2.0 (2024.06.11)
================================================== */

/* THEME
블랙 #333 
짙은 갈색 #806A5C (128,106,92)
연회색 #D5D0C7 (213,208,199)
*/

/* ==================== RESET =================== */

@font-face {
	font-family:'Pretendard-Regular'; 
	src:url('https://cdn.jsdelivr.net/gh/Project-Noonnu/noonfonts_2107@1.1/Pretendard-Regular.woff') format('woff'); 
	font-weight:400; 
	font-style:normal; 
}

@import url('https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght,SOFT@9..144,100..900,50&display=swap'); 

* {
	font-family:'Pretendard-Regular', sans-serif; 
	box-sizing:border-box; 
	padding:0; 
	margin:0; 
	border:none; 
	outline:none; 
}

.wrap select,
.wrap div,
.wrap button,
.wrap textarea,
.wrap span,
.wrap input,
.wrap img,
.wrap label{box-sizing:border-box; outline:0;}
.ui-dialog .charsheet {padding:0;}
.inlinerollresult{background-color:#FEF68E; border:2px solid #FEF68E; padding:0 3px 0 3px; font-weight:bold; cursor:help; font-size:1.1em}
.inlinerollresult.fullcrit{border:none;}
.inlinerollresult.fullfail{border:none;}
.inlinerollresult.importantroll{border:none;}
.inlineqroll{width:18px; height:18px; vertical-align:bottom}
.wrap p {margin:0;}
.wrap input[type="radio"],
.wrap input[type="checkbox"] {cursor:pointer;}
.wrap input[type="number"]::-webkit-outer-spin-button,
.wrap input[type="number"]::-webkit-inner-spin-button {appearance:none; -webkit-appearance:none; margin:0;}
.wrap select {cursor:pointer; appearance:none; -webkit-appearance:none; margin:0;}
.wrap button[type="roll"] {border:none; background:transparent; margin:0 !important; padding:0 !important; text-shadow:none !important; box-shadow:none !important;}
.wrap button:before {display:none !important;}
.repcontainer.editmode .repitem .itemcontrol {z-index:999;}

.charsheet div {box-sizing:border-box;}
.wrap input::placeholder {color:rgba(0,0,0,0.3); font-weight:400;}
.wrap textarea::placeholder {color:rgba(0,0,0,0.3); font-weight:400;}

.charsheet .repcontrol_add,
.charsheet .repcontrol_edit {
	border:none; 
	outline:none; 
	margin-top:5px;}

.charsheet .wrap .repcontrol {display:block; position:relative; overflow:hidden; border-radius:0; font-size:11px;}
.charsheet .wrap .repcontrol > * {height:20px; line-height:20px; padding:0 10px; font-size:11px; font-weight:400; color:#fff; background:#333; text-shadow:none;}
.charsheet .editor ~ * .sheet-group .repcontrol {display:none !important;}
.charsheet .editor:checked ~ * .sheet-group .repcontrol {display:block !important; margin-top:5px !important;}

/* ==================== GLOBAL =================== */

.charsheet .chk-circle {display:inline-block; position:relative; 
			left:0; width:12px; height:12px !important; 
			background-color:transparent; border-radius:50%; 
			border:1px solid #D5D0C7; 
			appearance:none !important; 
			-webkit-appearance:none; 
			outline:none !important; 
			cursor:pointer;}

.charsheet .chk-circle:checked {background-color:#D5D0C7;}

.charsheet .table button[type="roll"],
.charsheet .feat button[type="roll"],
.charsheet .spell button[type="roll"],
.charsheet .npc-dice button[type="roll"]{
    display:inline-block; vertical-align:middle; border:none; 
    padding:0 !important; 
    margin:0 !important; 
    width:20px; 
    height:26px; 
    border-radius:0; 
    background:url("https://l5ve.net/roll20/sheet/dnd/dice.png") no-repeat 50% 50%; 
    background-size:90% auto;}

.charsheet .feat-expand summary {
	display:inline-block; vertical-align:middle; border:none; 
    padding:0 !important; 
    margin:0 !important; 
    width:25px; 
    height:26px; 
    border-radius:0; 
    background:url("https://l5ve.net/roll20/sheet/dnd/expand.png") no-repeat 50% 50%; 
    background-size:70% auto; 
	cursor:pointer;}

.charsheet .feat-desc {
	padding:5px 0 5px 25px; 
	width:772px;}

.charsheet .feat-desc textarea {
	background-color:rgba(250,250,250,0.4); 
	width:100%; 
	height:100px; 
	padding:10px; 
	box-sizing:border-box; 
	text-align:left !important; 
	resize:none; 
	border:none; 
	outline:none; 
	border-left:5px solid #806A5C; 
	margin:0;}

.charsheet .spell-ready input[type=checkbox] {
	display: block;
	position: absolute;
	z-index: 2;
	height: 26px;
	width: 26px;
	opacity:0;}

.charsheet .spell-ready span {
	z-index:1; 
	background: url("https://l5ve.net/roll20/sheet/dnd/check.png") no-repeat 50% 50%;
	background-size: 90% auto;
	opacity: 0.2;
	cursor:pointer;
	position: relative;
	display: block;
	width: 18px;
	height: 18px;
	top: 5px;
	margin:0 auto;}

.charsheet .spell-ready input[type=checkbox]:checked + span {
	background-image:url("https://l5ve.net/roll20/sheet/dnd/check.png");
	opacity: 0.9;
	cursor:pointer;}

/* ==================== LAYOUT =================== */

.wrap { min-width: 840px; max-width: 840px; margin-bottom: 20px; }

/* Tabs */

.pc, .npc {display:none; 
			padding:20px; 
			background:#aca18f; 
			background-image:url("https://l5ve.net/roll20/sheet/dnd/background.png"), 
			url("https://l5ve.net/roll20/sheet/dnd/texture.png"),
			linear-gradient(to bottom, #aca18f, #b4aa99, #bbb2a4, #c3bbae, #cbc4b9, #cbc4b9, #cbc4b9, #cbc4b9, #c3bbae, #bbb2a4, #b4aa99, #aca18f); 
			background-repeat:no-repeat, repeat; 
			background-position:bottom; 
			height:auto;}
.tab[value="pc"] ~ div.pc, .tab[value="npc"] ~ div.npc {display:block;}
.tab_btn {margin-bottom:5px; z-index:999;}
.tab_btn button {display:inline-block; padding:2px 10px; border-radius:10px; 
				line-height:1.2; font-size:12px; 
				background-color:rgba(128,106,92,0.4);}


/* 경험치 */

.charsheet .exp {position:absolute; z-index: 200; width:168px; background-color:#806A5C; border-radius:15px;
	font-size:12px; top:95px; left:650px; padding:0 6px 2px 12px; height: 24px; }

.charsheet .exp span {display:inline-block; color:#d5d0c7; margin-right:5px; line-height: 24px; }
.charsheet .exp input[type="text"] {background:transparent; width: 52px; height: 24px; font-size:12px; border:none; color:#aca18f;}



/* ==================== EDITOR =================== */

.charsheet .editor {
	float:right; 
	height:20px !important; 
	width:46px; 
	border:none !important; 
	outline:none !important; 
	background:transparent; 
	appearance:none; 
	margin-top:-40px;
	margin-right: -20px;}

.charsheet .editor:checked::after {
	content:"EDIT"; 
	background:#333; 
	color:white; 
	padding:2px 10px; 
	border-radius:10px; 
	line-height:1.2; 
	outline:none !important; 
	font-size:12px;}

.charsheet .editor::after {
	content:"EDIT"; 
	background:rgba(128,106,92,0.4); 
	padding:2px 10px; 
	border-radius:10px; 
	line-height:1.2; 
	outline:none !important; 
	font-size:12px;}

.charsheet .editor {z-index:2;}
.charsheet .editor:checked ~ * .skill .edit {display:block !important; z-index:99;}
.charsheet .editor:checked ~ * .skill .read {display:none !important;}

.charsheet .edit-box {display:none;}
.charsheet .read-box {background-color:transparent; position:relative;}
.charsheet .editor:checked ~ * .skill .edit-box {display:block !important; color:#D5D0C7; z-index: 99; position: relative;}
.charsheet .editor:checked ~ * .skill .read-box {display:none !important;}
.charsheet .editor:checked ~ * header .left .shields .initiative button {display:none !important;}

/* ==================== HEADER =================== */

header {display:grid; 
		z-index:10; 
		grid-template-columns:210px auto 210px; 
		background-color:white; 
		height:330px; 
		border:2px solid #806A5C;
		border-radius:10px; 
		padding:20px;}

header .left {grid-column:1/2; z-index:20;}
header .left > div, header .right > div {display:inline-block;}
header .right {grid-column:3/-1; z-index:20; text-align:right;}
header .center {position:absolute; float:left; width:656px; height:400px; z-index:10; margin:-52px 0 0 50px; 
		background-image:url("https://l5ve.net/roll20/sheet/dnd/center.png"); 
		background-position:center; 
		background-repeat:no-repeat; 
		background-color:transparent;}

header div .h1 {margin-top:0; margin-bottom:10px;}
header div .h2 {margin-top:0; margin-bottom:10px;}
header span {display:block; font-size:12px; text-align:left; color:#D5D0C7; margin-bottom:-4px; cursor:default;}

header input[type="number"], 
header input[type="text"] {
	background:transparent; padding:0; font-weight:600; letter-spacing:-0.04em;  
	border:0; border-radius:0; border-bottom:1px solid #D5D0C7;}

header div .h1 input[type="number"],
header div .h1 input[type="text"] {width:100%; height:35px; font-size:18px;}

header div .h2 input[type="number"],
header div .h2 input[type="text"] {width:100%; height:28px; font-size:14px;}

header .level span, header .level input[type="text"] {text-align:center;}

/* Header Fields */

.name {width:195px;}
.race {width:125px;}
.age {width:45px; margin-left:5px;}
.gender {width:60px;}
.bmi {margin-left:5px; width:105px;}
.background {width:110px;}
.alignment {margin-left:5px; width:65px;}
.class {width:160px; margin-left:15px;}
.level {width:25px; margin-left:5px;}
.sub-class {width:175px; margin-left:35px;}
.pb {width:160px; height:47px; 
	position:relative; 
	background-image:url("https://l5ve.net/roll20/sheet/dnd/pb.png"); 
	background-repeat:no-repeat; background-color:transparent; 
	padding:12px 0 0 35px; 
	margin:2px 0 3px 40px;}
.pb span {width:65px; text-align:center;}
.pb input[type="number"] {width:50px !important; height:30px; margin-top:-30px; margin-left:-50px; 
	border:0 !important; font-size:16px; text-align:left;}
.hd {width:180px;}
.hd span {padding-left:10px;}
.hd input[type="text"] {padding-left:6px;}

/* Shields & HP */

header .left .shields {display:grid; 
		grid-template-columns:60px 60px 60px; 
		justify-content:space-between; 
		width:200px; height:80px; margin-left:-5px;}

.ac {grid-column:1/2; background-image:url("https://l5ve.net/roll20/sheet/dnd/ac.png"); background-repeat:no-repeat; background-color:transparent;}
.initiative {grid-column:2/3; background-image:url("https://l5ve.net/roll20/sheet/dnd/init.png"); background-repeat:no-repeat; background-color:transparent;}
.speed {grid-column:3/4; background-image:url("https://l5ve.net/roll20/sheet/dnd/speed.png"); background-repeat:no-repeat; background-color:transparent;}

.ac span, .initiative span, .speed span {width:60px; height:22px; text-align:center; font-size:11px; padding-top:20px; color:#D5D0C7;}
header .left .shields button[type="roll"] {background-color:#D5D0C7; height:15px; }

header .left .shields input[type="number"],
header .left .shields input[type="text"] {
	width:60px; height:55px; padding-top:5px; text-align:center; font-size:20px; 
}

header .left .shields input[type="number"],
header .left .shields input[type="text"] {border:none;}

header .left .shields .initiative button { z-index: 50; position:absolute; width:60px; height:60px; left:0; top:0; background-color:transparent; outline:none; appearance:none;}


header .right .hp {display:grid; 
		grid-template-columns:56px 56px 56px; 
		grid-template-rows:18px 30px; 
		justify-content:space-between; 
		width:192px; height:80px; margin:5px 0 0 20px; 
		padding:12px 10px 20px 10px; 
		background-image:url("https://l5ve.net/roll20/sheet/dnd/hp.png"); background-repeat:no-repeat; background-color:transparent;}

header .right .hp span {grid-row:1/2; text-align:center;}
header .right .hp input[type="number"] {width:56px; text-align:center; font-size:20px; border:none;}

/* Header Bottom */

.header_bottom {display:grid; 
				z-index:10; 
				grid-template-columns:15px 240px auto 240px 15px; 
				width:760px; 
				height:30px; 
				margin:0 auto; 
				padding-top:5px; 
				background-color:rgba(128,106,92,0.4); 
				border-radius:0 0 10px 10px;}

.header_bottom .left {grid-column:2/3; z-index:20; height:22px;}
.header_bottom .left span {display:inline-block; line-height:1.2;}

.header_bottom .right {grid-column:4/5; z-index:20; height:22px;}
.header_bottom .right span {margin:0 4px; height:20px;}
.header_bottom .right input[type="number"]{
	width:35px; 
	height:20px; 
	border:0; 
	padding:0 0 0 10px; 
	font-weight:800; 
	font-size:14px; 
	background-color:rgba(250,250,250,0.2);}

/* ==================== ABILITIES =================== */

.charsheet .ability { display: block; width: 800px; margin: 0 auto; text-align: center; }

.charsheet .ab-flex { display: inline-grid;
	grid-template-columns: repeat(6, 102px);
	grid-column-gap: 24px; 
	margin: 20px 0 20px 0; }

.charsheet .ab { text-align:center;}
.charsheet .ab > button {position:relative; z-index:15 !important; padding:4px 20px !important; background-color:#806A5C; 
	bottom:-2px; color:white; font-size:14px !important;}
.charsheet .ab .ab-mod {z-index:3; left:0; right:0; top:0; bottom:0; 
	margin:auto; width:72px; height:0; padding:36px 0; border:solid 2px #806A5C; 
	-ms-transform:rotate(45deg); -webkit-transform:rotate(45deg); transform:rotate(45deg); background-color:white;}
.charsheet .ab .ab-mod input {width:70%; height:50px; background:#FFF; 
	-ms-transform:translateY(-50%) rotate(-45deg); -webkit-transform:translateY(-50%) rotate(-45deg); transform:translateY(-50%) rotate(-45deg); 
	margin:auto; font-size:32px; font-weight:bold; text-align:center; background-color:transparent; border:0;}
.charsheet .ab .ab-score {z-index:10; left:0; right:0; top:0; bottom:0; margin:auto; margin-right:8px; margin-top:-26px; 
	width:30px; height:0; padding:16px 0; border:solid 1px #333; 
	-ms-transform:rotate(45deg); -webkit-transform:rotate(45deg); transform:rotate(45deg); background-color:#333;}
.charsheet .ab .ab-score input {width:90%; background:#FFF; 
	-ms-transform:translateY(-50%) rotate(-45deg); -webkit-transform:translateY(-50%) rotate(-45deg); transform:translateY(-50%) rotate(-45deg); 
	margin:auto; text-align:center; font-size:12px; background-color:transparent; border:0; color:#eee;}

/* ========== SKILLS ========== */

.charsheet .skill {display:block; position:relative;}
.charsheet .skill .prof-chk {z-index:1; display:block; position:absolute; left:0; width:13px; height:13px; 
	background-color:transparent; border-radius:50%; border:1px solid rgba(250,250,250,0.5); 
	appearance:none; -webkit-appearance:none; outline:none; cursor:pointer; top:8px;}

.charsheet .skill .prof-chk:checked {background-color:rgba(250,250,250,0.5);}
.charsheet .skill .edit {display:none !important;}

.charsheet .skill .name {display:block; position:relative; height:30px; margin-right:16px; line-height:30px; z-index:0;}

.charsheet .skill .name .read,
.charsheet .skill .name .edit {display:block; position:absolute; top:0; left:0; right:0; bottom:0;}
.charsheet .skill .custom-mod {display:block; position:absolute; right:0; top:-2px; width:20px; bottom:0; z-index:1; }
.charsheet .skill .custom-mod > input[type="number"].read {
	display:block; position:absolute; top:5px; right:0; left:0; 
	width:100%; height:20px; 
	background-color:transparent; 
	border:0; border-radius:0; border-bottom:1px solid rgba(0,0,0,0.3); 
	appearance:none; -webkit-appearance:none; outline:none; 
	text-align:center; font-size:13px; color:rgba(0,0,0,0.3);}

.charsheet .skill .custom-mod > input[type="number"].edit {display:block; position:absolute; 
	top:5px; right:0; left:0; width:100%; height:20px; text-align:center; }

.charsheet .skill .name button {display:block; position:relative; line-height:30px; 
	color:#333; font-size:13px; text-align:left; left:22px; width:60px; font-weight:600;}

/* ==================== SUB TABS - WEAPON ATTACK =================== */

.charsheet #common {display:grid; grid-template-columns:530px 250px; justify-content:space-between; margin-bottom:10px;}
.common-left {grid-column:1/2;}
.common-right {grid-column:2/3;}

.charsheet .tab-title {
	display:block; 
	margin:0 auto; 
	padding:5px; 
	text-align:center; 
	border-radius:10px; 
	background-color:#333; 
	font-size:14px; 
	margin-top:15px; 
	margin-bottom:5px; 
	color:white;}

.charsheet .table {
	display:grid; 
	grid-row-gap:3px; 
	grid-template-columns:140px 40px 125px 185px 25px; 
	margin-bottom:0; 
	justify-content:space-between;}

.table .title {font-size:13px; font-weight:800; text-align:center; padding:2px; }
.table .weapon-name {grid-column:1/2;}
.table .weapon-atk {grid-column:2/3;}
.table .weapon-dmg {grid-column:3/4;}
.table .weapon-prop {grid-column:4/5;}
.table .weapon-dice {grid-column:5/6;} 
.table input[type="text"] {border:none; text-align:center; background-color:rgba(250,250,250,0.4); width:100%; height:100%;}
.weapon-memo {grid-column:1/-1; resize:none; height:105px !important; 
			border:none; border-radius:5px; margin:1px 0 5px; font-size:14px; background-color:rgba(250,250,250,0.4); padding:10px;}

.charsheet .tab-header {
	display:inline-block; 
	position:absolute; 
	padding:3px 10px; 
	background-color:#806A5C; 
	color:white; 
	margin-top:-30px;}

.charsheet .tab-item {
	padding:10px; 
	padding-top:30px; 
	border:2px solid #806A5C; 
	background-color:white; 
	border-radius:10px; 
	margin-bottom:5px;}

.charsheet .tab-item:nth-child(3) {margin-bottom:0;}

.charsheet .tab-item textarea{
	resize:none; 
	width:100% !important; 
	padding:0 !important; 
	line-height:22px; 
	border:none; 
	border-radius:0; 
	background:repeating-linear-gradient(white, white 21px, #D5D0C7 22px, #D5D0C7 22px); 
	margin:0;}

.senses textarea {height:44px !important;}
.resistances textarea {height:44px !important;}
.proficiencies textarea {margin-top:-10px; height:110px !important;}
.items textarea {height:100% !important;}

.exhaustion {
	display:flex; 
	position:relative; 
	float:right; 
	width:80px; 
	height:20px; 
	margin-top:-22px; 
	flex-wrap:nowrap; 
	justify-content:flex-end; 
	align-items:stretch; 
}

.exhaustion input[type=number] {
	width:20px !important; 
	height:18px; 
	margin:0 2px; 
	border:none; 
	outline:none; 
	border-bottom:1px solid #D5D0C7; 
	border-radius:0; 
	text-align:center; 
}

.resistances input[type=text] {width:100%; height:16px; font-size:12px; color:#D5D0C7; 
	outline:none; border:none; padding:0; margin:0 auto; text-align:center; background-color:transparent; letter-spacing:-0.04rem;}

/* 상태 이상 */
.charsheet .conditions {display:flex; flex-direction:row; flex-wrap:wrap; justify-content:space-between; align-items:stretch; gap:5px 5px; width:100%;}
.charsheet .conditions-item {display:block; position:relative; width:70px; float:left; text-align:center; 
	line-height:26px; background-color:rgba(128,106,92,0.4); border-radius:5px; font-size:13px;}
.charsheet .conditions-item input[type="checkbox"] {display:block; position:absolute; height:26px; top:0; left:0; right:0; bottom:0; 
														z-index:2; opacity:0; margin:0; width:100%; height:100%; cursor:help;}
.charsheet .conditions-item input[type="checkbox"]:checked + span {border-radius:5px; background-color:#333; color:white; cursor:help;}
.charsheet .conditions-item span {display:block; position:relative; width:100%; height:26px;}

/* ==================== TOGGLES =================== */

.toggle-content {display:none;}
.toggle-menu[value="1"] + .toggle-content {display:block;}

.toggle {position:relative; z-index:0;}
.toggle > input[type="checkbox"] {display:block; position:absolute; top:0; left:0; right:0; bottom:0; width:100%; height:100%; z-index:2; 
							background-color:transparent; border:0; outline:0; appearance:none;}

.toggle > span:after {content:"➕"; display:block; position:absolute; right:10px; top:5px;}
.toggle > input[type="checkbox"]:checked + span:after,
.toggle > input[type="radio"]:checked + span:after {content:"➖";}

.toggle > .sheet-show-title:after {content:"➖"; display:block; position:absolute; right:10px; top:5px;}
.toggle > .sheet-toggle-show[type="checkbox"]:checked + .sheet-show-title:after,
.toggle > .sheet-toggle-show[type="radio"]:checked + .sheet-show-title:after {content:"➕";}

/* ==================== FEATS =================== */

.feat .feat-table, .feat .repitem {display:grid; grid-gap:3px; grid-template-columns:25px 55px 200px 65px 100px 310px 25px; 
	margin-bottom:0; justify-content:space-between;}
.feat .repitem {margin-top:3px;}

.feat-table .title {font-size:13px; font-weight:800; text-align:center; padding:2px; }

.feat .feat-expand {grid-column:1/2;}
.feat .feat-source {grid-column:2/3;}
.feat .feat-name {grid-column:3/4;}
.feat .feat-timing {grid-column:4/5;}
.feat .feat-limit {grid-column:5/6;}
.feat .feat-property {grid-column:6/7;}
.feat .feat-dice {grid-column:7/8;}

.feat input[type="text"], .feat select {border:none; text-align:center; background-color:rgba(250,250,250,0.4); width:100%; height:26px;}

/* ==================== SPELLS =================== */

/* 슬롯 테이블 */
.spell .spell-table, .spell .repitem {display:grid; grid-gap:3px; 
	grid-template-columns:25px 55px 20px 190px 65px 70px 65px 130px 65px 45px 25px; 
	margin-bottom:0; justify-content:space-between;}
.spell .repitem {margin-top:3px;}
.spell-table .title {font-size:13px; font-weight:800; text-align:center; padding:2px; }

.spell .spell-expand {grid-column:1/2;}
.spell .spell-lv {grid-column:2/3;}
.spell .spell-ready {grid-column:3/4;}
.spell .spell-name {grid-column:4/5;}
.spell .spell-timing {grid-column:5/6;}
.spell .spell-type {grid-column:6/7;}
.spell .spell-range {grid-column:7/8;}
.spell .spell-dmg {grid-column:8/9;}
.spell .spell-duration {grid-column:9/10;}
.spell .spell-vsm {grid-column:10/11;}
.spell  .spell-dice {grid-column:11/12;}

.spell input[type="text"], .spell select {border:none; text-align:center; background-color:rgba(250,250,250,0.4); width:100%; height:26px;}

/* 슬롯 디자인 */

.spells-slots {
	margin: auto;
    width: 64px;
    height: 0;
    padding: 30px 0;
    border: solid 2px rgba(128,106,92,0.8);
    -ms-transform: rotate(-45deg);
    -webkit-transform: rotate(-45deg);
    transform: rotate(-45deg);
    background-color: white;}

.spells-slots > span,
.spells-slots > input[type="number"],
.spells-slots > input[type="text"],
.spells-slots > select {
	display: block;
	-ms-transform: rotate(45deg);
    -webkit-transform: rotate(45deg);
    transform: rotate(45deg);}

.spells-slots > span {
	position: absolute;
	top: 12px;
	right:-2px;
	color: #D5D0C7;
	font-size: 12px;
	text-align: center;
	width: 48px;}

.spells-slots > input[type="number"],
.spells-slots > input[type="text"],
.spells-slots > select {
	background: transparent;
	border: none;
	outline: none;
	width: 55px !important;
	height: 30px !important;
	margin: 0;
	text-align: center;
	position: absolute;
	top: 20px;
	right: 8px;
	font-size: 24px;
	font-weight: 800;
	line-height: 24px;}

.spells-slots > select {font-size: 18px; line-height: 18px;}

.spell-slot-odd .spells-slots .chk-circle
	{margin-top: 36px;
	border: 1px solid #806A5C;
	background-color:rgba(128,106,92,0.4);}

.spell-slot-odd .spells-slots .chk-circle:checked
	{margin-top: 36px;
	border: 1px solid #806A5C;
	background-color:#333;}

.spell-slot-even .spells-slots .chk-circle
	{margin-top: -102px;
	border: 1px solid #806A5C;
	background-color:rgba(128,106,92,0.4);}

.spell-slot-even .spells-slots .chk-circle:checked
	{margin-top: -102px;
	border: 1px solid #806A5C;
	background-color:#333;}

.spell-slot-even {
	display: flex;
	justify-content: flex-start;
	width: 644px;
	margin-left: 46px;
	margin-bottom: -18px;}

.spell-slot-odd {
	display: flex;
	justify-content: flex-start;
	width: 736px;}

.spellslot {
	text-align: center;
	margin: 30px auto;
	width: 736px;}

.charsheet .spell-slot-odd > div:nth-child(1),
.charsheet .spell-slot-odd > div:nth-child(2),
.charsheet .spell-slot-odd > div:nth-child(8),
.charsheet .spell-slot-even > div:nth-child(1),
.charsheet .spell-slot-even > div:nth-child(7) {
	background-color:rgba(128,106,92,0.5);}

/* ==================== 인벤토리 =================== */

.inventory{
	display: grid;
	grid-template-columns: 265px repeat(7, 65px) 1px;
	grid-template-areas: "inv cp sp ep gp pp food rope il";
	grid-column-gap: 10px;
	margin-top: 15px; }

.cp { grid-area: cp; }
.sp { grid-area: sp; }
.ep { grid-area: ep; }
.gp { grid-area: gp; }
.pp { grid-area: pp; }
.food { grid-area: food; }
.rope { grid-area: rope; }
.inv { grid-area: inv; padding-right: 10px; }
.items { grid-column: 2/-1; grid-row:1/-1; margin-top: 90px; }

.inv .inv-table, .inv .repitem {display:grid; grid-gap:3px; grid-template-columns: 18px 150px 30px 45px; 
	margin-bottom:0; justify-content:space-between;}
.inv .repitem {margin-top:3px;}

.inv-table .title {font-size:13px; font-weight:800; text-align:center; padding:2px; }

.inv .inv-ready {grid-column:1/2;}
.inv .inv-name {grid-column:2/3;}
.inv .inv-amount {grid-column:3/4;}
.inv .inv-weight {grid-column:4/5;}
.inv .inv-weight {grid-column:4/5;}

.inv input[type="text"], .inv select {border:none; text-align:center; background-color:rgba(250,250,250,0.4); width:100%; height:26px;}

.div-circle {
	height: 65px;
	width: 65px;
	background-color:white;
	border-radius: 50%;
	border: solid 2px #806A5C;
	margin: 10px 0 0 0;}

.div-circle:nth-child(6),
.div-circle:nth-child(7) {
	background-color:rgba(128,106,92,0.3);
	color: white;}

.div-circle span:first-child {
	display: inline-block;
	position: relative;
	background-color: #806A5C;
	color: white;
	width: 30px;
	height: 18px;
	text-align: center;
	top: -8px;
	left: 16px;
	line-height: 18px;
	cursor: help;
	font-size: 11px;}

.div-circle span:first-child::before {
	content: "";
	position: absolute;
	top: -6px;
	left: 0;
	width: 0;
	height: 0;
	border-left: 15px solid transparent;
	border-right: 15px solid transparent;
	border-bottom: 6px solid #806A5C;
	}

.div-circle span:first-child::after{
content: "";
  position: absolute;
  bottom: -6px;
  left: 0;
  width: 0;
  height: 0;
  border-left: 15px solid transparent;
  border-right: 15px solid transparent;
  border-top: 6px solid #806A5C;
}

.div-circle input {
	display: block;
	width: 52px !important;
	height: 20px !important;
	outline: none;
	border: none;
	background-color: transparent;
	text-align: center;
	line-height: 20px;
	font-weight: 800;
	font-size: 16px;
	margin: 0 auto;}

.div-circle span:last-child {
	display: block;
	font-size: 11px;
	color: #D5D0C7;
	text-align: center;
	margin-top: -2px;}

/* 하단 메모 */

.charsheet .memo {
	display:block; 
	outline:none; 
	border:none; 
	height:230px; 
	resize:none; 
	background-color:rgba(250,250,250,0.4); 
	border-radius:10px; 
	margin:20px 0 0 0; 
	padding:20px; 
	box-sizing:border-box; 
	font-size:14px;}

/* ==================== ROLL TEMPLATES =================== */

.sheet-rolltemplate-custom,
.sheet-rolltemplate-npc {display:block; position:relative; 
	width:92%; max-width:500px; 
	border:10px solid transparent; border-right:0; border-left:0;}

.sheet-rolltemplate-custom > div,
.sheet-rolltemplate-npc > div {background-color:white; padding:12px; box-sizing:border-box; border-radius:0 20px 0 20px;}

.sheet-rolltemplate-custom::before,
.sheet-rolltemplate-custom::after,
.sheet-rolltemplate-npc::before,
.sheet-rolltemplate-npc::after {position:absolute; display:block; width:50px; height:50px; content:"";}

.sheet-rolltemplate-custom::before {left:-8px; top:-8px; border-left:8px solid #333; border-top:8px solid #6d8e6b;}
.sheet-rolltemplate-custom::after {right:-8px; bottom:-8px; border-right:8px solid #333; border-bottom:8px solid #6d8e6b;}

.sheet-rolltemplate-npc::before {left:-8px; top:-8px; border-left:8px solid #333; border-top:8px solid #871A16;}
.sheet-rolltemplate-npc::after {right:-8px; bottom:-8px; border-right:8px solid #333; border-bottom:8px solid #871A16;}


.sheet-rolltemplate-custom .sheet-rth > span:nth-child(1),
.sheet-rolltemplate-npc .sheet-rth > span:nth-child(1) {display:block; color:#B2B2B2; font-size:11px;}

.sheet-rolltemplate-custom .sheet-rth > span:nth-child(2),
.sheet-rolltemplate-npc .sheet-rth > span:nth-child(2){display:block; font-size:16px; font-weight:800; line-height:1.6;}

.sheet-rolltemplate-custom .sheet-rtb,
.sheet-rolltemplate-npc .sheet-rtb {border-top:1px solid #DBDBDB; margin-top:4px;}

.sheet-rolltemplate-custom .inlinerollresult, 
.sheet-rolltemplate-npc .inlinerollresult {font-family:'Fraunces', serif; font-weight:700; text-align:center; 
	background-color:transparent; border:0; cursor:help; font-size:24px;}
.sheet-rolltemplate-custom .sheet-dmg,
.sheet-rolltemplate-npc .sheet-dmg {font-size:14px; margin-top:2px; padding:3px; background-color:#eee;}
.sheet-rolltemplate-custom .sheet-dmg span,
.sheet-rolltemplate-npc .sheet-dmg span {font-family:'Pretendard-Regular', sans-serif; font-size: 14px; padding: 0;}

.sheet-rolltemplate-custom .inlinerollresult.fullcrit,
.sheet-rolltemplate-npc .inlinerollresult.fullcrit {border:0 !important; color:#3fb315;}
.sheet-rolltemplate-custom .inlinerollresult.fullfail,
.sheet-rolltemplate-npc .inlinerollresult.fullfail {border:0 !important; color:#b31515;}
.sheet-rolltemplate-custom .inlinerollresult.importantroll,
.sheet-rolltemplate-npc .inlinerollresult.importantroll {border:0 !important; color:#4a57ed;}
.sheet-rolltemplate-custom .sheet-grey, 
.sheet-rolltemplate-npc .sheet-grey {color:#C6C6C6;}

.sheet-rolltemplate-custom .sheet-tag,
.sheet-rolltemplate-npc .sheet-tag {display:inline-block; background-color:#DBDBDB; border-radius:2px; 
								padding:3px 5px; font-size:11px; font-weight:400; margin-top:4px;}
.sheet-rolltemplate-custom .sheet-tag span,
.sheet-rolltemplate-npc .sheet-tag span {font-family:'Pretendard-Regular', sans-serif; font-size: 11px; padding: 0; }

.sheet-rolltemplate-custom .sheet-hl,
.sheet-rolltemplate-npc .sheet-hl {background-color:#333; color:white;}

.sheet-rolltemplate-custom .sheet-desc, 
.sheet-rolltemplate-npc .sheet-desc {color:#B2B2B2; font-size:11px; font-weight: 400; padding:5px 0 2px 0;}
.sheet-rolltemplate-custom .sheet-desc span,
.sheet-rolltemplate-npc .sheet-desc span {font-family:'Pretendard-Regular', sans-serif; font-size: 11px; padding: 0;}


/* ==================== NPC =================== */

.dice-control { display: block; float: right; position: relative; 
	height: 20px; margin-top:-44px; margin-right: -20px;}

.dice-control > label {
	display: inline-block;
	width: 45px; height: 12px;
	font-size: 12px;
	font-weight: 400;}

.dice-control > label > input[type="radio"] {
	background-color: rgba(128,106,92,0.4);
	width: 45px;
	height: 25px;}

.dice-control > label > span {
	background-color: rgba(128,106,92,0.4);
	display: inline-block;
	position: relative;
	top: -25px;
	width: 45px;
	height: 20px;
	border-radius: 50px;
	text-align: center;
	cursor: pointer;}

.dice-control > label > input[type="radio"] {
	background-color: rgba(128,106,92,0.4);
	width: 45px;
	height: 25px;
	opacity: 0;}

.dice-control > label > input[type="radio"]:checked + span {
	background-color: #333;
	color: white;}

.npc-header { 
	width: 100%;
	background-color: white; padding: 15px;
	border: 2px solid #806A5C;
	border-radius: 10px; 
	display: grid; 
	grid-gap: 2px; 
	grid-template-columns: 220px 27px 145px 33px 150px 180px ;
	grid-template-rows: 18px 34px 18px;}

.npc-header .npc-name { grid-column: 1/2; }
.npc-header .npc-ac { grid-column: 2/3; }
.npc-header .npc-hp { grid-column: 3/4; }
.npc-header .npc-init { grid-column: 4/5; }
.npc-header .npc-speed { grid-column: 5/6; }
.npc-header .npc-senses { grid-column: 6/7; }
.npc-header .npc-tag{ grid-row: 3/4; grid-column: 1/4; }
.npc-header .npc-cr { grid-row: 3/4; grid-column: 6/7; text-align: right; }

.npc-header span { font-size: 12px; color: #D5D0C7; } 
.npc-header > input[type="text"],
.npc-header > button > input[type="text"] { width: 100%; border: 0; outline:0; appearance: none; border-radius:4px; 
	font-size: 16px; height: 34px; font-weight: 800;}

.npc-header input[readonly] {width: 100%; height: 18px; font-size: 12px; color: #D5D0C7; 
							border: 0; outline: none; background-color: transparent; cursor:help; }

.npc > button[type="roll"] { position: absolute; height: 55px; width: 33px; left: 455px; top: 120px; }

/* NPC 능력치 */

.npc-section { display: grid;
	grid-template-columns: 240px 140px auto;
	grid-column-gap: 10px; }

.npc-section button[type="roll"] {	display: inline-block; padding: 4px 10px !important;
    background-color: #806A5C; color: white; font-size: 13px !important; }

.npc-section input[type="number"],
.npc-section input[type="text"] {
	width: 40px !important;
	height: 26px;
	text-align: center;
	font-size: 14px;
	background-color: rgba(250,250,250,0.4);
	border: none; outline: none;}

.npc-section input:read-only {
	display: inline-block;
	width:18px !important; height: 30px; padding: 0; margin: 0; outline: none; border: none; border-radius: 0; 
	background-color: transparent; text-align: center;
	font-size: 13px;}


/* NPC 기술 */

.npc-skill > select {
	width: 70px !important;
	height: 26px; 
	padding: 4px 10px;
	background-color:#806A5C;
	color: white;
	font-size: 13px;
	border: none;
	outline: none;
	text-align: center;}

.npc-dice { display: inline-block; width: 22px; height:30px; text-align:center; }


/* NPC 저항, 면역, 언어 */

.npc-data { display: grid;
	grid-template-columns: 64px auto;
	height: 28px;
	margin-bottom: 6px; }

.npc-data span { grid-column: 1/2; border: 2px solid #333; padding-left: 2px;  
				border-radius: 5px 0 0 5px; background-color:#333; color: #fff; text-align: center; line-height: 26px; }
.npc-data textarea 	{ 
	background-color:rgba(250,250,250,0.4);
	border-radius: 0 5px 5px 0;
	width:100%; 
	height:100%; 
	padding-left:10px; 
	resize:none; 
	outline:none; 
	margin:0;}

.npc-skill-summary { background-color:rgba(250,250,250,0.4); 
	width: 400px;
	height: 80px !important;
	margin: 0;
	outline: none; resize: none; border: 0; padding: 10px; }

/* NPC 능력 */

.npc .npc-table, .npc .repitem {
	display:grid; grid-gap:3px; grid-template-columns:25px 70px 200px 100px 40px 100px 220px 25px; 
	margin-bottom:0; 
	border:1px;}
.npc-table .repitem {margin-top:3px;}

.npc-table .title {font-size:13px; font-weight:800; text-align:center; padding:2px; }

.npc-table .npc-expand {grid-column:1/2;}
.npc-table .npc-source {grid-column:2/3;}
.npc-table .npc-name {grid-column:3/4;}
.npc-table .npc-limit {grid-column:4/5;}
.npc-table .npc-attack {grid-column:5/6;}
.npc-table .npc-range {grid-column:6/7;}
.npc-table .npc-damage {grid-column:7/8;}
.npc-table .npc-dice {grid-column:8/9;}

.npc-table input[type="text"],
.npc .repitem input[type="text"] {border:none; text-align:center; background-color:rgba(250,250,250,0.4); width:100%; height:26px;}

.npc-table > select,
.npc .repitem > select {
	padding: 4px 10px;
	background-color:rgba(250,250,250,0.4);
	width: 100%;
	height: 26px;
	border: none;
	outline: none;
	text-align: center;}
  </style>
</head>
<body>
 <div class="wrap">
	<!-- Tabs -->
	<input type="hidden" class="tab" name="attr_tab" value="pc">
	<div class="tab_btn">
		<button type="action" name="act_pc">PC</button>
		<button type="action" name="act_npc">NPC</button>
	</div>
	<input type="hidden" name="attr_chat" value="?{공개 여부|전체,|GM에게,/w gm }">
	<input type="hidden" name="attr_d20" value="1d20">
	<input type="hidden" name="attr_mod" value="1">
	<!-- PC -->
	<div class="pc">
		<input type="checkbox" class="editor" value="1" title="매뉴얼 수정치">
		<div class="exp">
			<span>EXP</span>
			<input type="text" name="attr_exp" placeholder="경험치" value="0"/>
			<span>/</span>
			<input type="hidden" name="attr_foo_exp" value="0" />
			<input type="text" name="attr_exp_max" title="다음 레벨까지 필요한 경험치" value="@{foo_exp}" disabled="true" readonly />
		</div>
		<header>
			<div class="left">
				<div class="name h1">
					<span>캐릭터 이름</span>
					<input type="text" name="attr_character_name" value="">
				</div>
				<div class="race h2">
					<span>종족</span>
					<input type="text" name="attr_race" value="">
				</div>
				<div class="age h2">
					<span>나이</span>
					<input type="text" name="attr_age" value="">
				</div>
				<div class="gender h2">
					<span>성별</span>
					<input type="text" name="attr_gender" value="">
				</div>
				<div class="bmi h2">
					<span>키・몸무게</span>
					<input type="text" name="attr_bmi" value="">
				</div>
				<div class="background h2">
					<span>배경</span>
					<input type="text" name="attr_background" value="">
				</div>
				<div class="alignment h2">
					<span>성향</span>
					<input type="text" name="attr_alignment" value="">
				</div>
				<div class="shields">
					<div class="ac">
						<span>AC</span>
						<div class="skill">
							<input type="number" name="attr_ac_custom" class="edit-box" value="10">
							<input type="number" name="attr_ac" class="read-box" value="@{ac_custom} + @{shield_check}"
								disabled="true" readonly>
						</div>
						<input type="checkbox" class="chk-circle" style="left:-5px; top:-8px;" title="AC에 방패 추가"
							name="attr_shield_check" value="2">
					</div>
					<div class="initiative">
						<span>우선권</span>
						<div class="skill">
							<input type="number" name="attr_initiative_custom" class="edit-box" value="0">
							<input type="number" name="attr_initiative" class="read-box"
								value="@{initiative_custom}@{dex_mod}" disabled="true" readonly>
							<!-- 우선권 추가 버튼 -->
							<button type="roll" class="dice_initiative" title="토큰 선택 후 클릭"
								value="&{template:custom} {{name=@{character_name}}} {{subject=우선권}} {{atk=[[@{mod}]]}} {{r1=[[@{d20} + @{initiative} &{tracker}]]}} {{query=1}} {{normal=1}} {{r2=[[0d20+@{initiative}]]}}"></button>
						</div>
					</div>
					<div class="speed">
						<span>속도</span>
						<input type="text" name="attr_speed" placeholder="30ft" value="30ft">
					</div>
				</div>
			</div>
			<div class="center"></div>
			<div class="right">
				<div class="class h1">
					<span>클래스</span>
					<input type="text" name="attr_class" value="">
				</div>
				<div class="level h1">
					<span>레벨</span>
					<input type="text" name="attr_level" value="1">
				</div>
				<div class="sub-class h2">
					<span>서브 클래스</span>
					<input type="text" name="attr_sub_class" value="">
				</div>
				<div class="pb">
					<span>숙련 보너스</span>
					<input type="number" name="attr_pb" class="read" value="ceil((@{level}/4)+1)" disabled="true"
						readonly>
				</div>
				<div class="hd h2">
					<span>히트 다이스</span>
					<input type="text" name="attr_hd" value="" placeholder="현재 합계">
				</div>
				<div class="hp">
					<span>현재</span>
					<input type="number" name="attr_hp" value="0">
					<span>최대</span>
					<input type="number" name="attr_hp_max" value="0">
					<span>임시</span>
					<input type="number" name="attr_hp_temp" value="0">
				</div>
			</div>
		</header>
		<div class="header_bottom">
			<div class="left">
				<!-- 죽음 내성 -->
				<span>죽음 내성 ✦</span>
				<span style="margin: 0 4px;">성공</span>
				<input type="checkbox" class="chk-circle" name="attr_ds-s1" value="1">
				<input type="checkbox" class="chk-circle" name="attr_ds-s2" value="1">
				<input type="checkbox" class="chk-circle" name="attr_ds-s3" value="1">
				<span style="margin: 0 4px 0 8px;">실패</span>
				<input type="checkbox" class="chk-circle" name="attr_ds-f1" value="1">
				<input type="checkbox" class="chk-circle" name="attr_ds-f2" value="1">
				<input type="checkbox" class="chk-circle" name="attr_ds-f3" value="1">
			</div>
			<div class="right">
				<!-- 상시 -->
				<span>상시 감지</span>
				<input type="number" name="attr_pperception" placeholder="0" disabled="true" value="@{perception} +10"
					readonly>
				<span>통찰</span>
				<input type="number" name="attr_pinsight" placeholder="0" disabled="true" value="@{insight} +10"
					readonly>
				<span>수사</span>
				<input type="number" name="attr_pinvestigation" placeholder="0" disabled="true"
					value="@{investigation} +10" readonly>
			</div>
		</div>
		<!-- Stats -->
		<div class="ability">
			<div class="ab-flex">
				<!-- STRENGTH -->
				<div class="ab">
					<button type="roll"
						value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=근력}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}@{str_mod}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}@{str_mod}]]}}">근력</button>
					<div class="ab-mod"><input readonly name="attr_str_mod" value="+0"></div>
					<div class="ab-score"><input type="number" name="attr_str" value="10"></div>
					<div style="display: block; margin-bottom: 20px;"></div>
					<!-- 근력 내성-->
					<div class="skill">
						<input type="checkbox" name="attr_str_save_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=근력 내성}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{str_save}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{str_save}]]}}">근력
								내성</button>
						</div>
						<div class="custom-mod">
							<input type="number" name="attr_str_save_custom" class="edit" value="0">
							<input type="number" name="attr_str_save" class="read"
								value="floor(@{str_mod} + @{str_save_custom} + @{str_save_check})" disabled="true"
								readonly>
						</div>
					</div>
					<!-- 운동 -->
					<div class="skill">
						<input type="checkbox" name="attr_athletics_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=운동}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{athletics}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{athletics}]]}}">운동</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_athletics_custom" class="edit" value="0">
							<input type="number" name="attr_athletics" class="read"
								value="floor(@{str_mod} + @{athletics_custom} + @{athletics_check})" disabled="true"
								readonly>
						</div>
					</div>
				</div>
				<!-- DEXTERITY -->
				<div class="ab">
					<button type="roll" class="dice"
						value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=민첩}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}@{dex_mod}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}@{dex_mod}]]}}">민첩</button>
					<div class="ab-mod"><input readonly name="attr_dex_mod" title="@{dex_mod}" value="+0"></div>
					<div class="ab-score"><input type="number" name="attr_dex" value="10"></div>
					<div style="display: block; margin-bottom: 20px;"></div>
					<!-- 민첩 내성-->
					<div class="skill">
						<input type="checkbox" name="attr_dex_save_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=민첩 내성}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{dex_save}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{dex_save}]]}}">민첩
								내성</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_dex_save_custom" class="edit" value="0">
							<input type="number" name="attr_dex_save" class="read"
								value="floor(@{dex_mod} + @{dex_save_custom} + @{dex_save_check})" disabled="true"
								readonly>
						</div>
					</div>
					<!-- 곡예 -->
					<div class="skill">
						<input type="checkbox" name="attr_acrobatics_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=곡예}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{acrobatics}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{acrobatics}]]}}">곡예</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_acrobatics_custom" class="edit" value="0">
							<input type="number" name="attr_acrobatics" class="read"
								value="floor(@{dex_mod} + @{acrobatics_custom} + @{acrobatics_check})" disabled="true"
								readonly>
						</div>
					</div>
					<!-- 손속임 -->
					<div class="skill">
						<input type="checkbox" name="attr_sleight_of_hand_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=손속임}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{sleight_of_hand}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{sleight_of_hand}]]}}">손속임</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_sleight_of_hand_custom" class="edit" value="0">
							<input type="number" name="attr_sleight_of_hand" class="read"
								value="floor(@{dex_mod} + @{sleight_of_hand_custom} + @{sleight_of_hand_check})"
								disabled="true" readonly>
						</div>
					</div>
					<!-- 은신 -->
					<div class="skill">
						<input type="checkbox" name="attr_stealth_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=은신}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{stealth}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{stealth}]]}}">은신</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_stealth_custom" class="edit" value="0">
							<input type="number" name="attr_stealth" class="read"
								value="floor(@{dex_mod} + @{stealth_custom} + @{stealth_check})" disabled="true"
								readonly>
						</div>
					</div>
				</div>
				<!-- CONSTITUTION -->
				<div class="ab">
					<button type="roll" class="dice"
						value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=건강}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}@{con_mod}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}@{con_mod}]]}}">건강</button>
					<div class="ab-mod"><input readonly name="attr_con_mod" value="+0"></div>
					<div class="ab-score"><input type="number" name="attr_con" value="10"></div>
					<div style="display: block; margin-bottom: 20px;"></div>
					<!-- 건강 내성-->
					<div class="skill">
						<input type="checkbox" name="attr_con_save_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=건강 내성}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{con_save}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{con_save}]]}}">건강
								내성</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_con_save_custom" class="edit" value="0">
							<input type="number" name="attr_con_save" class="read"
								value="floor(@{con_mod} + @{con_save_custom} + @{con_save_check})" disabled="true"
								readonly>
						</div>
					</div>
				</div>
				<!-- INTELLIGENCE -->
				<div class="ab">
					<button type="roll" class="dice"
						value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=지능}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}@{int_mod}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}@{int_mod}]]}}">지능</button>
					<div class="ab-mod"><input readonly name="attr_int_mod" value="+0"></div>
					<div class="ab-score"><input type="number" name="attr_int" value="10"></div>
					<div style="display: block; margin-bottom: 20px;"></div>
					<!-- 지능 내성-->
					<div class="skill">
						<input type="checkbox" name="attr_int_save_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=지능 내성}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{int_save}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{int_save}]]}}">지능
								내성</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_int_save_custom" class="edit" value="0">
							<input type="number" name="attr_int_save" class="read"
								value="floor(@{int_mod} + @{int_save_custom} + @{int_save_check})" disabled="true"
								readonly>
						</div>
					</div>
					<!-- 비전학 -->
					<div class="skill">
						<input type="checkbox" name="attr_arcana_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=비전학}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{arcana}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{arcana}]]}}">비전학</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_arcana_custom" class="edit" value="0">
							<input type="number" name="attr_arcana" class="read"
								value="floor(@{int_mod} + @{arcana_custom} + @{arcana_check})" disabled="true" readonly>
						</div>
					</div>
					<!-- 수사 -->
					<div class="skill">
						<input type="checkbox" name="attr_investigation_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=수사}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{investigation}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{investigation}]]}}">수사</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_investigation_custom" class="edit" value="0">
							<input type="number" name="attr_investigation" class="read"
								value="floor(@{int_mod} + @{investigation_custom} + @{investigation_check})"
								disabled="true" readonly>
						</div>
					</div>
					<!-- 역사학 -->
					<div class="skill">
						<input type="checkbox" name="attr_history_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=역사학}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{history}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{history}]]}}">역사학</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_history_custom" class="edit" value="0">
							<input type="number" name="attr_history" class="read"
								value="floor(@{int_mod} + @{history_custom} + @{history_check})" disabled="true"
								readonly>
						</div>
					</div>
					<!-- 자연학 -->
					<div class="skill">
						<input type="checkbox" name="attr_nature_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=자연학}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{nature}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{nature}]]}}">자연학</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_nature_custom" class="edit" value="0">
							<input type="number" name="attr_nature" class="read"
								value="floor(@{int_mod} + @{nature_custom} + @{nature_check})" disabled="true" readonly>
						</div>
					</div>
					<!-- 종교학 -->
					<div class="skill">
						<input type="checkbox" name="attr_religion_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=종교학}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{religion}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{religion}]]}}">종교학</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_religion_custom" class="edit" value="0">
							<input type="number" name="attr_religion" class="read"
								value="floor(@{int_mod} + @{religion_custom} + @{religion_check})" disabled="true"
								readonly>
						</div>
					</div>
				</div>
				<!-- WISDOM -->
				<div class="ab">
					<button type="roll" class="dice"
						value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=지혜}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}@{wis_mod}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}@{wis_mod}]]}}">지혜</button>
					<div class="ab-mod"><input readonly name="attr_wis_mod" value="+0"></div>
					<div class="ab-score"><input type="number" name="attr_wis" value="10"></div>
					<div style="display: block; margin-bottom: 20px;"></div>
					<!-- 지혜 내성-->
					<div class="skill">
						<input type="checkbox" name="attr_wis_save_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=지혜 내성}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{wis_save}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{wis_save}]]}}">지혜
								내성</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_wis_save_custom" class="edit" value="0">
							<input type="number" name="attr_wis_save" class="read"
								value="floor(@{wis_mod} + @{wis_save_custom} + @{wis_save_check})" disabled="true"
								readonly>
						</div>
					</div>
					<!-- 감지 -->
					<div class="skill">
						<input type="checkbox" name="attr_perception_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=감지}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{perception}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{perception}]]}}">감지</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_perception_custom" class="edit" value="0">
							<input type="number" name="attr_perception" class="read"
								value="floor(@{wis_mod} + @{perception_custom} + @{perception_check})" disabled="true"
								readonly>
						</div>
					</div>
					<!-- 동물 조련 -->
					<div class="skill">
						<input type="checkbox" name="attr_animal_handling_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=동물 조련}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{animal_handling}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{animal_handling}]]}}">동물
								조련</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_animal_handling_custom" class="edit" value="0">
							<input type="number" name="attr_animal_handling" class="read"
								value="floor(@{wis_mod} + @{animal_handling_custom} + @{animal_handling_check})"
								disabled="true" readonly>
						</div>
					</div>
					<!-- 생존 -->
					<div class="skill">
						<input type="checkbox" name="attr_survival_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=생존}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{survival}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{survival}]]}}">생존</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_survival_custom" class="edit" value="0">
							<input type="number" name="attr_survival" class="read"
								value="floor(@{wis_mod} + @{survival_custom} + @{survival_check})" disabled="true"
								readonly>
						</div>
					</div>
					<!-- 의학 -->
					<div class="skill">
						<input type="checkbox" name="attr_medicine_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=의학}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{medicine}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{medicine}]]}}">의학</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_medicine_custom" class="edit" value="0">
							<input type="number" name="attr_medicine" class="read"
								value="floor(@{wis_mod} + @{medicine_custom} + @{medicine_check})" disabled="true"
								readonly>
						</div>
					</div>
					<!-- 통찰 -->
					<div class="skill">
						<input type="checkbox" name="attr_insight_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=통찰}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{insight}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{insight}]]}}">통찰</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_insight_custom" class="edit" value="0">
							<input type="number" name="attr_insight" class="read"
								value="floor(@{wis_mod} + @{insight_custom} + @{insight_check})" disabled="true"
								readonly>
						</div>
					</div>
				</div>
				<!-- CHARISMA -->
				<div class="ab">
					<button type="roll" class="dice"
						value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=매력}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}@{cha_mod}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}@{cha_mod}]]}}">매력</button>
					<div class="ab-mod"><input readonly name="attr_cha_mod" value="+0"></div>
					<div class="ab-score"><input type="number" name="attr_cha" value="10"></div>
					<div style="display: block; margin-bottom: 20px;"></div>
					<!-- 매력 내성-->
					<div class="skill">
						<input type="checkbox" name="attr_cha_save_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=매력 내성}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{cha_save}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{cha_save}]]}}">매력
								내성</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_cha_save_custom" class="edit" value="0">
							<input type="number" name="attr_cha_save" class="read"
								value="floor(@{cha_mod} + @{cha_save_custom} + @{cha_save_check})" disabled="true"
								readonly>
						</div>
					</div>
					<!-- 공연 -->
					<div class="skill">
						<input type="checkbox" name="attr_performance_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=공연}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{performance}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{performance}]]}}">공연</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_performance_custom" class="edit" value="0">
							<input type="number" name="attr_performance" class="read"
								value="floor(@{cha_mod} + @{performance_custom} + @{performance_check})" disabled="true"
								readonly>
						</div>
					</div>
					<!-- 기만 -->
					<div class="skill">
						<input type="checkbox" name="attr_deception_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=기만}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{deception}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{deception}]]}}">기만</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_deception_custom" class="edit" value="0">
							<input type="number" name="attr_deception" class="read"
								value="floor(@{cha_mod} + @{deception_custom} + @{deception_check})" disabled="true"
								readonly>
						</div>
					</div>
					<!-- 설득 -->
					<div class="skill">
						<input type="checkbox" name="attr_persuasion_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=설득}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{persuasion}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{persuasion}]]}}">설득</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_persuasion_custom" class="edit" value="0">
							<input type="number" name="attr_persuasion" class="read"
								value="floor(@{cha_mod} + @{persuasion_custom} + @{persuasion_check})" disabled="true"
								readonly>
						</div>
					</div>
					<!-- 위협 -->
					<div class="skill">
						<input type="checkbox" name="attr_intimidation_check" class="prof-chk" value="@{pb}">
						<div class="name">
							<button type="roll" class="dice"
								value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=위협}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}+@{intimidation}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}+@{intimidation}]]}}">위협</button>
						</div>
						<div class="edit"></div>
						<div class="custom-mod">
							<input type="number" name="attr_intimidation_custom" class="edit" value="0">
							<input type="number" name="attr_intimidation" class="read"
								value="floor(@{cha_mod} + @{intimidation_custom} + @{intimidation_check})"
								disabled="true" readonly>
						</div>
					</div>
				</div>
				<!--능력치 기술 종료-->
			</div>
		</div>
		<section id="common">
			<!-- 무기 공격, 상태 이상-->
			<div class="common_left">
				<span class="tab-title" style="background-color:#806A5C; color: white; margin: 0 0 5px 0;">무기 공격</span>
				<div class="table">
					<div class="weapon-name title">이름</div>
					<div class="weapon-atk title">명중</div>
					<div class="weapon-dmg title">피해</div>
					<div class="weapon-prop title">속성</div>
					<div class="weapon-dice">&nbsp;</div>
					<!--무기 공격 1-->
					<input type="text" class="weapon-name" name="attr_weapon_name_01" value="" placeholder="단검">
					<input type="text" class="weapon-atk" name="attr_weapon_atk_01" value="" placeholder="+3">
					<input type="text" class="weapon-dmg" name="attr_weapon_dmg_01" value="" placeholder="[[1d3+1]] 관통">
					<input type="text" class="weapon-prop" name="attr_weapon_prop_01" value=""
						placeholder="교묘함, 경량, 투척 (거리 20/60)">
					<button type="roll" class="weapon-dice" class="dice-icon"
						value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=@{weapon_name_01}}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}@{weapon_atk_01}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}@{weapon_atk_01}]]}} {{dmg=@{weapon_dmg_01}}} {{desc=@{weapon_prop_01}}}"></button>
					<!--무기 공격 2-->
					<input type="text" class="weapon-name" name="attr_weapon_name_02" value="">
					<input type="text" class="weapon-atk" name="attr_weapon_atk_02" value="">
					<input type="text" class="weapon-dmg" name="attr_weapon_dmg_02" value="">
					<input type="text" class="weapon-prop" name="attr_weapon_prop_02" value="">
					<button type="roll" class="weapon-dice" class="dice-icon"
						value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=@{weapon_name_02}}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}@{weapon_atk_02}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}@{weapon_atk_02}]]}} {{dmg=@{weapon_dmg_02}}} {{desc=@{weapon_prop_02}}}"></button>
					<!-- 무기 공격 3 -->
					<input type="text" class="weapon-name" name="attr_weapon_name_03" value="">
					<input type="text" class="weapon-atk" name="attr_weapon_atk_03" value="">
					<input type="text" class="weapon-dmg" name="attr_weapon_dmg_03" value="">
					<input type="text" class="weapon-prop" name="attr_weapon_prop_03" value="">
					<button type="roll" class="weapon-dice" class="dice-icon"
						value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=@{weapon_name_03}}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}@{weapon_atk_03}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}@{weapon_atk_03}]]}} {{dmg=@{weapon_dmg_03}}} {{desc=@{weapon_prop_03}}}"></button>
					<!-- 무기 공격 4-->
					<input type="text" class="weapon-name" name="attr_weapon_name_04" value="">
					<input type="text" class="weapon-atk" name="attr_weapon_atk_04" value="">
					<input type="text" class="weapon-dmg" name="attr_weapon_dmg_04" value="">
					<input type="text" class="weapon-prop" name="attr_weapon_prop_04" value="">
					<button type="roll" class="weapon-dice" class="dice-icon"
						value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=@{weapon_name_04}}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}@{weapon_atk_04}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}@{weapon_atk_04}]]}} {{dmg=@{weapon_dmg_04}}} {{desc=@{weapon_prop_04}}}"></button>
					<!-- 무기 공격 5-->
					<input type="text" class="weapon-name" name="attr_weapon_name_05" value="">
					<input type="text" class="weapon-atk" name="attr_weapon_atk_05" value="">
					<input type="text" class="weapon-dmg" name="attr_weapon_dmg_05" value="">
					<input type="text" class="weapon-prop" name="attr_weapon_prop_05" value="">
					<button type="roll" class="weapon-dice" class="dice-icon"
						value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=@{weapon_name_05}}} {{atk=[[@{mod}]]}} {{r1=[[@{d20}@{weapon_atk_05}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}@{weapon_atk_05}]]}} {{dmg=@{weapon_dmg_05}}} {{desc=@{weapon_prop_05}}}"></button>
					<!-- 메모 & 고양감 -->
					<textarea name="attr_weapon_memo" class="weapon-memo" placeholder="" value="" />
				</div>
				<!-- 상태 이상 -->
				<div class="conditions">
					<div class="conditions-item">
						<input type="checkbox" name="attr_frightened" class="condition-chk" value="1"
							title="• 공포의 대상이 시선 내 있을 때 모든 능력 판정과 명중 굴림에 불리점 &#13;• 자의로 공포의 대상에게 가까이 다가갈 수 없음">
						<span>공포</span>
					</div>
					<div class="conditions-item">
						<input type="checkbox" name="attr_prone" class="condition-chk" value="1"
							title="• 일어서지 않는 한 포복으로만 이동 가능 &#13;• 이동의 절반을 사용해 일어날 시 상태 종료 &#13;• 넘어진 상태로 공격 시 명중 굴림에 불리점 &#13;• 5ft 이내 위치한 넘어진 상대를 공격 시 명중에 이점 &#13;• 5ft 이상 떨어진 곳에서 넘어진 상대를 공격 시 명중에 불리점 &#13;">
						<span>넘어짐</span>
					</div>
					<div class="conditions-item">
						<input type="checkbox" name="attr_paralyzed" class="condition-chk" value="1"
							title="• 【행동 불능】 상태 동시 적용 &#13;• 이동 불가능, 대화 불가능 &#13;• 근력 및 민첩 내성 굴림 자동 실패 &#13;• 마비된 상대를 공격 시 명중에 이점 &#13;• 마비된 상대를 대상으로 5ft 이내 공격 명중 시 치명타 처리 &#13;">
						<span>마비</span>
					</div>
					<div class="conditions-item">
						<input type="checkbox" name="attr_charmed" class="condition-chk" value="1"
							title="• 매혹자를 상대로 공격, 해로운 능력 및 마법 효과 사용 불가능 &#13;• 매혹자는 매혹 상태를 상대로 사회적 교류 시 모든 능력 판정에 이점 &#13;">
						<span>매혹</span>
					</div>
					<div class="conditions-item">
						<input type="checkbox" name="attr_unconsious" class="condition-chk" value="1"
							title="• 【행동 불능】, 【넘어짐】  상태 동시 적용 &#13;• 손에 들고 있는 것을 떨어트림 &#13;• 근력 및 민첩 내성 굴림 자동 실패 &#13;• 무의식 상태의 상대를 공격 시 명중에 이점 &#13;• 무의식 상태의 상대를 대상으로 5ft 이내 공격 명중 시 치명타 처리 &#13;">
						<span>무의식</span>
					</div>
					<div class="conditions-item">
						<input type="checkbox" name="attr_grappled" class="condition-chk" value="1"
							title="• 이동 속도 0, 속도에 어떤 추가 행동도 받을 수 없음 &#13;• 붙잡힌 상태로 【행동 불능】이 되면 상태 자동 종료 &#13;• 강제 이동 효과로 인해 간격에서 벗어날 경우 상태 자동 종료 &#13;">
						<span>붙잡힘</span>
					</div>
					<div class="conditions-item">
						<input type="checkbox" name="attr_petrified" class="condition-chk" value="1"
							title="• 【행동 불능】 상태 동시 적용 &#13;• 이동 불가능, 대화 불가능, 주변 환경 인지 불가능 &#13;• 장비 및 들고 있던 모든 비마법적인 물건 역시 무기물로 변함 &#13;• 무게 10배 증가, 나이를 먹지 않음 &#13;• 석화된 상대를 공격 시 명중에 이점 &#13;• 근력 및 민첩 내성 굴림 자동 실패 &#13;• 모든 피해 저항 &#13;• 독과 질병에 면역 (석화되기 전에 있던 독이나 질병이 사라지지는 않음) &#13;">
						<span>석화</span>
					</div>
					<div class="conditions-item">
						<input type="checkbox" name="attr_blinded" class="condition-chk" value="1"
							title="• 볼 수 없음 &#13;• 시각과 관련된 모든 능력 판정 자동 실패 &#13;• 시각이 손실된 상대를 공격 시 명중에 이점 &#13;• 시각이 손실된 상태로 공격 시 명중에 불리점">
						<span>시력 상실</span>
					</div>
					<div class="conditions-item">
						<input type="checkbox" name="attr_poisoned" class="condition-chk" value="1"
							title="• 모든 명중 굴림과 능력 판정에 불리점">
						<span>중독</span>
					</div>
					<div class="conditions-item">
						<input type="checkbox" name="attr_deafened" class="condition-chk" value="1"
							title="• 들을 수 없음 &#13;• 청각과 관련된 모든 능력 판정 자동 실패 &#13;">
						<span>청각 상실</span>
					</div>
					<div class="conditions-item">
						<input type="checkbox" name="attr_stunned" class="condition-chk" value="1"
							title="• 【행동 불능】 상태 동시 적용 &#13;• 이동 불가능, 말도 더듬거리기만 가능 &#13;• 근력 및 민첩 내성 굴림 자동 실패 &#13;• 충격받은 상대를 공격 시 명중에 이점 &#13;">
						<span>충격</span>
					</div>
					<div class="conditions-item">
						<input type="checkbox" name="attr_invisible" class="condition-chk" value="1"
							title="• 마법이나 특별한 감각의 도움 없이 볼 수 없음 &#13;• 은신 가능 여부를 판정할 때 심하게 가려져 있는 상태로 취급 &#13;• 투명한 상대 공격 시 명중에 불리점 &#13;• 투명한 상태로 공격 시 명중에 이점 &#13;">
						<span>투명</span>
					</div>
					<div class="conditions-item">
						<input type="checkbox" name="attr_restrained" class="condition-chk" value="1"
							title="• 이동 속도 0, 속도에 어떤 추가 행동도 받을 수 없음 &#13;• 포박된 상대 공격 시 명중에 이점 &#13;• 포박된 상태로 공격 시 명중에 불리점 &#13;• 민첩 내성 굴림에 불리점 &#13;">
						<span>포박</span>
					</div>
					<div class="conditions-item">
						<input type="checkbox" name="attr_incapacitated" class="condition-chk" value="1"
							title="• 행동 및 반응 행동 불가능">
						<span>행동 불능</span>
					</div>
				</div>
			</div>
			<!--감각, 이동, 상태, 숙련 -->
			<div class="common-right">
				<div class="tab-item senses">
					<span class="tab-header" style="cursor:help"
						title="• 특별한 이동 속도를 가지고 있지 않을 시 등반 및 수영 시 1ft마다 추가 1ft 소모&#13;&#13;• 멀리뛰기&#13;　　최소 10ft의 도움닫기 시 근력 1점당 1ft&#13;　　제자리에 서서 멀리 뛸 경우 근력 점수의 절반 ft&#13;　　뛰는 거리만큼 이동력 소모&#13;　　어려운 지형에 착지 시 DC10 곡예 판정에 성공해야 제대로 착지함&#13;&#13;•  높이 뛰기 &#13;　　최소 10ft의 도움닫기 시 근력 수정치 + 3ft&#13;　　제자리에서 높이 뛸 경우 ↑ 높이의 절반 ft&#13;　　뛰어오른 거리만큼 이동력 소모&#13;　　위로 닿을 수 있는 최대 높이는 높이뛰기 높이 + 키 x 1.5ft">감각
						& 이동</span>
					<textarea name="attr_senses" placeholder="암시야, 비행 및 수영 속도 등등" value="" />
				</div>
				<div class="tab-item resistances">
					<span class="tab-header">상태 & 저항</span>
					<div class="exhaustion">
						<span>탈진</span>
						<input type="number" name="attr_exhaustion" value="0">
						<span>단계</span>
					</div>
					<textarea name="attr_resistances" placeholder="저항, 면역, 취약성 등등" value="" />
					<input type="text" name="attr_exh_desc" value="@{foo_exh_desc}" title="탈진 단계에 따른 상태 이상"
						disabled="true" readonly>
				</div>
				<div class="tab-item proficiencies">
					<span class="tab-header">숙련</span>
					<textarea name="attr_proficiencies" placeholder="갑옷 및 방패&#13;무기&#13;도구&#13;언어&#13;기타" value="" />
				</div>
			</div>
			<!-- Common Area End -->
		</section>
		<!-- 피트 토글 박스-->
		<div class="toggle">
			<input type="checkbox" name="attr_toggle_feat" value="1">
			<span class="tab-title">종족 특성, 클래스 요소 및 재주</span>
		</div>
		<input type="hidden" name="attr_toggle_feat" class="toggle-menu" value="" />
		<div class="toggle-content feat">
			<!-- 피트 테이블-->
			<div class="feat-table">
				<div class="feat-expand title">&nbsp;</div>
				<div class="feat-source title">출처</div>
				<div class="feat-name title">이름</div>
				<div class="feat-timing title">사용</div>
				<div class="feat-limit title">사용 제한</div>
				<div class="feat-property title">참고</div>
				<div class="feat-dice title">&nbsp;</div>
				<!--피트 1-->
				<details class="feat-expand">
					<summary></summary>
					<p class="feat-desc"><textarea name="attr_feat_desc_01" value="" placeholder="효과 설명" /></p>
				</details>
				<select class="feat-source" name="attr_feat_source_01">
					<option value></option>
					<option value="종족">종족</option>
					<option value="클래스">클래스</option>
					<option value="재주">재주</option>
					<option value="기타">기타</option>
				</select>
				<input class="feat-name" type="text" name="attr_feat_name_01" value="" />
				<select class="feat-timing" name="attr_feat_timing_01">
					<option value></option>
					<option value="행동">행동</option>
					<option value="추가 행동">추가 행동</option>
					<option value="반응 행동">반응 행동</option>
					<option value="상시">상시</option>
					<option value="기타">기타</option>
				</select>
				<input class="feat-limit" type="text" name="attr_feat_limit_01" value="" />
				<input type="text" class="feat-property" name="attr_feat_prop_01" value=""
					placeholder="사거리, DC, 피해 등" />
				<button class="feat-dice" type="roll" class="dice-icon"
					value="&{template:custom}{{name=@{character_name}}} {{subject=@{feat_name_01}}}{{timing=@{feat_timing_01}}}{{limit=@{feat_limit_01}}}{{prop=@{feat_prop_01}}}{{desc=@{feat_desc_01}}}"></button>
				<!--피트 2-->
				<details class="feat-expand">
					<summary></summary>
					<p class="feat-desc"><textarea name="attr_feat_desc_02" value="" placeholder="효과 설명" /></p>
				</details>
				<select class="feat-source" name="attr_feat_source_02">
					<option value></option>
					<option value="종족">종족</option>
					<option value="클래스">클래스</option>
					<option value="재주">재주</option>
					<option value="기타">기타</option>
				</select>
				<input class="feat-name" type="text" name="attr_feat_name_02" value="" />
				<select class="feat-timing" name="attr_feat_timing_02">
					<option value></option>
					<option value="행동">행동</option>
					<option value="추가 행동">추가 행동</option>
					<option value="반응 행동">반응 행동</option>
					<option value="상시">상시</option>
					<option value="기타">기타</option>
				</select>
				<input class="feat-limit" type="text" name="attr_feat_limit_02" value="" />
				<input type="text" class="feat-property" name="attr_feat_prop_02" value="" />
				<button class="feat-dice" type="roll" class="dice-icon"
					value="&{template:custom}{{name=@{character_name}}} {{subject=@{feat_name_02}}}{{timing=@{feat_timing_02}}}{{limit=@{feat_limit_02}}}{{prop=@{feat_prop_02}}}{{desc=@{feat_desc_02}}}"></button>
				<!--피트 3-->
				<details class="feat-expand">
					<summary></summary>
					<p class="feat-desc"><textarea name="attr_feat_desc_03" value="" placeholder="효과 설명" /></p>
				</details>
				<select class="feat-source" name="attr_feat_source_03">
					<option value></option>
					<option value="종족">종족</option>
					<option value="클래스">클래스</option>
					<option value="재주">재주</option>
					<option value="기타">기타</option>
				</select>
				<input class="feat-name" type="text" name="attr_feat_name_03" value="" />
				<select class="feat-timing" name="attr_feat_timing_03">
					<option value></option>
					<option value="행동">행동</option>
					<option value="추가 행동">추가 행동</option>
					<option value="반응 행동">반응 행동</option>
					<option value="상시">상시</option>
					<option value="기타">기타</option>
				</select>
				<input class="feat-limit" type="text" name="attr_feat_limit_03" value="" />
				<input type="text" class="feat-property" name="attr_feat_prop_03" value="" />
				<button class="feat-dice" type="roll" class="dice-icon"
					value="&{template:custom}{{name=@{character_name}}} {{subject=@{feat_name_03}}}{{timing=@{feat_timing_03}}}{{limit=@{feat_limit_03}}}{{prop=@{feat_prop_03}}}{{desc=@{feat_desc_03}}}"></button>
			</div>
			<!-- Repeating Feature -->
			<fieldset class="repeating_feat">
				<details class="feat-expand">
					<summary></summary>
					<p class="feat-desc"><textarea name="attr_feat_desc" value="" placeholder="효과 설명" /></p>
				</details>
				<select class="feat-source" name="attr_feat_source">
					<option value></option>
					<option value="종족">종족</option>
					<option value="클래스">클래스</option>
					<option value="재주">재주</option>
					<option value="기타">기타</option>
				</select>
				<input class="feat-name" type="text" name="attr_feat_name" value="" />
				<select class="feat-timing" name="attr_feat_timing">
					<option value></option>
					<option value="행동">행동</option>
					<option value="추가 행동">추가 행동</option>
					<option value="반응 행동">반응 행동</option>
					<option value="상시">상시</option>
					<option value="기타">기타</option>
				</select>
				<input class="feat-limit" type="text" name="attr_feat_limit" value="" />
				<input type="text" class="feat-property" name="attr_feat_prop" value="" />
				<button class="feat-dice" type="roll" class="dice-icon"
					value="&{template:custom}{{name=@{character_name}}} {{subject=@{feat_name}}}{{timing=@{feat_timing}}}{{limit=@{feat_limit}}}{{prop=@{feat_prop}}}{{desc=@{feat_desc}}}"></button>
			</fieldset>
		</div>
		<div class="toggle">
			<input type="checkbox" name="attr_toggle_spell" value="1">
			<span class="tab-title">주문</span>
		</div>
		<input type="hidden" name="attr_toggle_spell" class="toggle-menu" value="" />
		<div class="toggle-content spell">
			<!-- 주문 내용 시작 -->
			<!-- 주문 슬롯 -->
			<div class="spellslot">
				<div class="spell-slot-even">
					<div class="spells-slots slot-dark">
						<span>명중</span>
						<input type="text" name="attr_spell_atk" value="" placeholder="+5"/>
					</div>
					<div class="spells-slots">
						<span>소마법</span>
						<input type="number" class="slots" name="attr_slot0" value="">
					</div>
					<div class="spells-slots">
						<span>2 레벨</span>
						<input type="number" class="slots" name="attr_slot2" value="">
						<input type="checkbox" class="chk-circle" name="attr_2a" value="1"/>
						<input type="checkbox" class="chk-circle" name="attr_2b" value="1"/>
						<input type="checkbox" class="chk-circle" name="attr_2c" value="1"/>
					</div>
					<div class="spells-slots">
						<span>4 레벨</span>
						<input type="number" class="slots" name="attr_slot4" value="">
						<input type="checkbox" class="chk-circle" name="attr_4a" value="1"/>
						<input type="checkbox" class="chk-circle" name="attr_4b" value="1"/>
						<input type="checkbox" class="chk-circle" name="attr_4c" value="1"/>
					</div>
					<div class="spells-slots">
						<span>6 레벨</span>
						<input type="number" class="slots" name="attr_slot6" value="">
						<input type="checkbox" class="chk-circle" name="attr_6a" value="1"/>
						<input type="checkbox" class="chk-circle" name="attr_6b" value="1"/>
					</div>
					<div class="spells-slots">
						<span>8 레벨</span>
						<input type="number" class="slots" name="attr_slot8" value="">
						<input type="checkbox" class="chk-circle" name="attr_8a" value="1"/>
					</div>
					<div class="spells-slots">
						<span>아는 주문</span>
						<input type="text" class="slots" name="attr_maxknown" value="">
					</div>
				</div>
				<div class="spell-slot-odd">
					<div class="spells-slots">
						<span>능력치</span>
						<select name="attr_spell_mod">
							<option value></option>
							<option value="@{int_mod}">지능</option>
							<option value="@{wis_mod}">지혜</option>
							<option value="@{cha_mod}">매력</option>
						</select>
					</div>
					<div class="spells-slots">
						<span>DC</span>
						<input type="number" name="attr_spell_dc" value=""/>
					</div>
					<div class="spells-slots">
						<span>1 레벨</span>
						<input type="number" class="slots" name="attr_slot1" value="">
						<input type="checkbox" class="chk-circle" name="attr_1a" value="1"/>
						<input type="checkbox" class="chk-circle" name="attr_1b" value="1"/>
						<input type="checkbox" class="chk-circle" name="attr_1c" value="1"/>
						<input type="checkbox" class="chk-circle" name="attr_1d" value="1"/>
					</div>
					<div class="spells-slots">
						<span>3 레벨</span>
						<input type="number" class="slots" name="attr_slot3" value="">
						<input type="checkbox" class="chk-circle" name="attr_3a" value="1"/>
						<input type="checkbox" class="chk-circle" name="attr_3b" value="1"/>
						<input type="checkbox" class="chk-circle" name="attr_3c" value="1"/>
					</div>
					<div class="spells-slots">
						<span>5 레벨</span>
						<input type="number" class="slots" name="attr_slot5" value="">
						<input type="checkbox" class="chk-circle" name="attr_5a" value="1"/>
						<input type="checkbox" class="chk-circle" name="attr_5b" value="1"/>
						<input type="checkbox" class="chk-circle" name="attr_5c" value="1"/>
					</div>
					<div class="spells-slots">
						<span>7 레벨</span>
						<input type="number" class="slots" name="attr_slot7" value="">
						<input type="checkbox" class="chk-circle" name="attr_7a" value="1"/>
						<input type="checkbox" class="chk-circle" name="attr_7b" value="1"/>
					</div>
					<div class="spells-slots">
						<span>9 레벨</span>
						<input type="number" class="slots" name="attr_slot9" value="">
						<input type="checkbox" class="chk-circle" name="attr_9a" value="1"/>
					</div>
					<div class="spells-slots">
						<span>준비 가능</span>
						<input type="number" class="slots" name="attr_maxprep" value="">
					</div>
				</div>
			</div>
			<!-- 주문 테이블 -->
			<div class="spell-table">
				<div class="spell-expand title">&nbsp;</div>
				<div class="spell-lv title">레벨</div>
				<div class="spell-ready title"></div>
				<div class="spell-name title">이름</div>
				<div class="spell-timing title">시전</div>
				<div class="spell-type title">명중/DC</div>
				<div class="spell-range title">사거리</div>
				<div class="spell-dmg title">피해</div>
				<div class="spell-duration title">지속 시간</div>
				<div class="spell-vsm title">구성</div>
				<div class="spell-dice title">&nbsp;</div>
				<!-- 주문 -->
				<details class="feat-expand spell-expand">
					<summary></summary>
					<p class="feat-desc"><textarea name="attr_spell_desc_01" value="" placeholder="효과 설명" /></p>
				</details>
				<select class="spell-lv" name="attr_spell_lv_01">
					<option value></option>
					<option value="0">캔트립</option>
					<option value="1">레벨 1</option>
					<option value="2">레벨 2</option>
					<option value="3">레벨 3</option>
					<option value="4">레벨 4</option>
					<option value="5">레벨 5</option>
					<option value="6">레벨 6</option>
					<option value="7">레벨 7</option>
					<option value="8">레벨 8</option>
					<option value="9">레벨 9</option>
					<option value="10">기타</option>
				</select>
				<div class="spell-ready"><input type="checkbox" name="attr_ready_01" value="1" title="준비"/><span></span></div>
				<input class="spell-name" type="text" name="attr_spell_name_01" value="" />
				<input class="spell-timing" type="text" name="attr_spell_timing_01" value="" placeholder="행동/R" />
				<select class="spell-type" name="attr_spell_type_01">
					<option value="7"></option>
					<option value="0">주문 명중</option>
					<option value="1">근력 내성</option>
					<option value="2">민첩 내성</option>
					<option value="3">건강 내성</option>
					<option value="4">지능 내성</option>
					<option value="5">지혜 내성</option>
					<option value="6">매력 내성</option>
					<option value="7">기타</option>
				</select>
				<input class="spell-range" type="text" name="attr_spell_range_01" value="" />
				<input class="spell-dmg" type="text" name="attr_spell_dmg_01" value="" />
				<input class="spell-duration" type="text" name="attr_spell_duration_01" value="" placeholder="1분/C" />
				<input class="spell-vsm" type="text" name="attr_spell_vsm_01" value="" placeholder="VSM" />
				<button class="spell-dice" type="roll" class="dice-icon"
					value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=@{spell_name_01}}} {{timing=@{spell_timing_01}}} {{range=@{spell_range_01}}} {{dc=@{foo_spl_dc}}} {{duration=@{spell_duration_01}}} {{vsm=@{spell_vsm_01}}} {{atk=[[@{foo_spl_atk}]]}} {{r1=[[@{d20}@{foo_spl_atk}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}@{foo_spl_atk}]]}} {{dmg=@{spell_dmg_01}}} {{desc=@{spell_desc_01}}}"></button>
			</div>
			<!-- Repeating Spells -->
			<fieldset class="repeating_spell">
				<details class="feat-expand spell-expand">
					<summary></summary>
					<p class="feat-desc"><textarea name="attr_spell_desc" value="" placeholder="효과 설명" /></p>
				</details>
				<select class="spell-lv" name="attr_spell_lv">
					<option value></option>
					<option value="0">캔트립</option>
					<option value="1">레벨 1</option>
					<option value="2">레벨 2</option>
					<option value="3">레벨 3</option>
					<option value="4">레벨 4</option>
					<option value="5">레벨 5</option>
					<option value="6">레벨 6</option>
					<option value="7">레벨 7</option>
					<option value="8">레벨 8</option>
					<option value="9">레벨 9</option>
					<option value="10">기타</option>
				</select>
				<div class="spell-ready"><input type="checkbox" name="attr_ready_01" value="1" title="준비"/><span></span></div>
				<input class="spell-name" type="text" name="attr_spell_name" value="" />
				<input class="spell-timing" type="text" name="attr_spell_timing" value="" placeholder="행동/R" />
				<input class='hidden' type='text' name='attr_rep_atk' readonly />
				<input class='hidden' type='text' name='attr_rep_dc' readonly />
				<select class="spell-type" name="attr_spell_type">
					<option value="7"></option>
					<option value="0">주문 명중</option>
					<option value="1">근력 내성</option>
					<option value="2">민첩 내성</option>
					<option value="3">건강 내성</option>
					<option value="4">지능 내성</option>
					<option value="5">지혜 내성</option>
					<option value="6">매력 내성</option>
					<option value="7">기타</option>
				</select>
				<input class="spell-range" type="text" name="attr_spell_range" value="" />
				<input class="spell-dmg" type="text" name="attr_spell_dmg" value="" />
				<input class="spell-duration" type="text" name="attr_spell_duration" value="" placeholder="1분/C" />
				<input class="spell-vsm" type="text" name="attr_spell_vsm" value="" placeholder="VSM" />
				<button class="spell-dice" type="roll" class="dice-icon"
					value="@{chat}&{template:custom} {{name=@{character_name}}} {{subject=@{spell_name}}} {{timing=@{spell_timing}}} {{range=@{spell_range}}} {{dc=@{rep_dc}}} {{duration=@{spell_duration}}} {{vsm=@{spell_vsm}}} {{atk=[[@{rep_atk}]]}} {{r1=[[@{d20}@{rep_atk}]]}} ?{주사위|기본,{{query=1&amp;#125;&amp;#125; {{normal=1&amp;#125;&amp;#125; {{r2=[[0d20|이점,{{query=1&amp;#125;&amp;#125; {{advantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;|불리점,{{query=1&amp;#125;&amp;#125; {{disadvantage=1&amp;#125;&amp;#125; {{r2=[[@{d20&#125;}@{rep_atk}]]}} {{dmg=@{spell_dmg}}} {{desc=@{spell_desc}}}"></button>
			</fieldset>
			<!-- 주문 내용 종료 -->
		</div>
		<div class="toggle">
			<input type="checkbox" name="attr_toggle_inventory" value="1">
			<span class="tab-title">장비 및 모험 도구</span>
		</div>
		<input type="hidden" name="attr_toggle_inventory" class="toggle-menu" value="" />
		<div class="toggle-content">
			<!--동전-->
			<div class="inventory">
				<div class="div-circle cp">
					<span title="• 1/10 SP &#13;• 1/50 EP &#13;• 1/100 GP &#13;• 1/1000 PP">CP</span>
					<input type="number" name="attr_cp" value="0"/>
					<span>동화</span>
				</div>
				<div class="div-circle sp">
					<span title="• 10 CP &#13;• 1/5 EP &#13;• 1/10 GP &#13;• 1/100 PP">SP</span>
					<input type="number" name="attr_sp" value="0"/>
					<span>은화</span>
				</div>
				<div class="div-circle ep">
					<span title="• 50 CP &#13;• 5 SP &#13;• 1/2 GP &#13;• 1/20 PP ">EP</span>
					<input type="number" name="attr_ep" value="0"/>
					<span>호박금화</span>
				</div>
				<div class="div-circle gp">
					<span title="• 100 CP &#13;• 10 SP &#13;• 2 EP &#13;• 1/10 PP ">GP</span>
					<input type="number" name="attr_gp" value="0"/>
					<span>금화</span>
				</div>
				<div class="div-circle pp">
					<span title="• 1000 CP &#13;• 100 SP &#13;• 20 EP &#13;• 10 GP ">PP</span>
					<input type="number" name="attr_pp" value="0"/>
					<span>백금화</span>
				</div>
				<div class="div-circle food">
					<span>식량</span>
					<input type="number" name="attr_ration" value="0"/>
					<span>1일치</span>
				</div>
				<div class="div-circle rope">
					<span>로프</span>
					<input type="number" name="attr_rope" value="0"/>
					<span>ft</span>
				</div>
				<div class="tab-item items">
					<span class="tab-header">기타 아이템</span>
					<textarea name="attr_items" value="" />
				</div>
				<div class="inv">
					<span class="tab-title" style="background-color:#806A5C; color: white; margin: 0 0 5px 0;">착용 장비・아이템</span>
					<div class="inv-table">
						<div class="inv-ready title"></div>
						<div class="inv-name title">이름</div>
						<div class="inv-amount title">#</div>
						<div class="inv-weight title">무게</div>
						<!--아이템 1-->
						<div class="spell-ready inv-ready"><input type="checkbox" name="attr_equip_01" value="1" title="장비함"/><span></span></div>
						<input class="inv-name" type="text" name="attr_inv_name_01" value="" />
						<input class="inv-amount" type="text" name="attr_inv_amount_01" value=""/>
						<input type="text" class="inv-weight" name="attr_inv_weight_01" value="" />
						<!--아이템 2-->
						<div class="spell-ready inv-ready"><input type="checkbox" name="attr_equip_02" value="1" title="장비함"/><span></span></div>
						<input class="inv-name" type="text" name="attr_inv_name_02" value="" />
						<input class="inv-amount" type="text" name="attr_inv_amount_02" value=""/>
						<input type="text" class="inv-weight" name="attr_inv_weight_02" value="" />
						<!--아이템 3-->
						<div class="spell-ready inv-ready"><input type="checkbox" name="attr_equip_03" value="1" title="장비함"/><span></span></div>
						<input class="inv-name" type="text" name="attr_inv_name_03" value="" />
						<input class="inv-amount" type="text" name="attr_inv_amount_03" value=""/>
						<input type="text" class="inv-weight" name="attr_inv_weight_03" value="" />
						<!--아이템 4-->
						<div class="spell-ready inv-ready"><input type="checkbox" name="attr_equip_04" value="1" title="장비함"/><span></span></div>
						<input class="inv-name" type="text" name="attr_inv_name_04" value="" />
						<input class="inv-amount" type="text" name="attr_inv_amount_04" value=""/>
						<input type="text" class="inv-weight" name="attr_inv_weight_04" value="" />
						<!--아이템 5-->
						<div class="spell-ready inv-ready"><input type="checkbox" name="attr_equip_05" value="1" title="장비함"/><span></span></div>
						<input class="inv-name" type="text" name="attr_inv_name_05" value="" />
						<input class="inv-amount" type="text" name="attr_inv_amount_05" value=""/>
						<input type="text" class="inv-weight" name="attr_inv_weight_05" value="" />
						<!--아이템 6-->
						<div class="spell-ready inv-ready"><input type="checkbox" name="attr_equip_06" value="1" title="장비함"/><span></span></div>
						<input class="inv-name" type="text" name="attr_inv_name_06" value="" />
						<input class="inv-amount" type="text" name="attr_inv_amount_06" value=""/>
						<input type="text" class="inv-weight" name="attr_inv_weight_06" value="" />
						<!-- 반복되는 아이템--> 
					</div>
					<fieldset class="repeating_inventory">
						<div class="spell-ready inv-ready"><input type="checkbox" name="attr_equip" value="1" title="장비함"/><span></span></div>
						<input class="inv-name" type="text" name="attr_inv_name" value="" />
						<input class="inv-amount" type="text" name="attr_inv_amount" value=""/>
						<input type="text" class="inv-weight" name="attr_inv_weight" value="" />
					</fieldset>
				</div>
			</div>
		</div>
		<textarea name="attr_memo" class="memo" placeholder="" value="" />
	</div>
	<!-- NPC -->
	<div class="npc compendium-drop-target">
		<div class="dice-control">
		<label class="sheet-radio">
			<input type="radio" name="attr_bonus" value="{{query=1}} {{advantage=1}} {{r2=[[@{d20}">
			<span title="@{bonus}">이점</span>
		</label>
		<label class="sheet-radio">
			<input type="radio" name="attr_bonus" value="{{query=1}} {{normal=1}} {{r2=[[0d20" checked>
			<span title="@{bonus}">기본</span>
		</label>
		<label class="sheet-radio">
			<input type="radio" name="attr_bonus" value="{{query=1}} {{disadvantage=1}} {{r2=[[@{d20}">
			<span title="@{bonus}">불리점</span>
		</label>
		<label class="sheet-radio">
			<input type="radio" name="attr_whisper" value="" checked>
			<span title="@{whisper}">전체</span>
		</label>
		<label class="sheet-radio">
			<input type="radio" name="attr_whisper" value="/w gm ">
			<span title="@{whisper}">귓속말</span>
		</label>
		</div>
		<div class="npc-header">
			<span class="npc-name">이름</span>
			<span class="npc-ac">AC</span>
			<span class="npc-hp">HP</span>
			<span class="npc-init">우선권</span>
			<span class="npc-speed">속도</span>
			<span class="npc-senses">감각</span>
			<input class="npc-name" type="text" name="attr_character_name" accept="Name" value=""/>
			<input class="npc-ac" type="text" name="attr_npc_ac" accept="AC" value=""/>
			<input class="npc-hp" type="text" name="attr_npc_hp" accept="HP" value=""/>
			<input type="text" name="attr_npc_init" value="@{npc_dex_mod}" disabled="true"/>
			<input class="npc-speed" type="text" name="attr_npc_speed" accept="Speed" value=""/>
			<input class="npc-senses" type="text" name="attr_npc_senses" accept="Senses" value=""/>
			<!-- hidden values-->
			<input type="hidden" name="attr_npc_size" accept="Size" value="크기"/>
			<input type="hidden" name="attr_npc_type" accept="Type" value="종류"/>
			<input type="hidden" name="attr_npc_alignment" accept="Alignment" value="성향"/>
			<input type="hidden" name="attr_npc_challenge" accept="Challenge Rating" value=""/>
			<!-- end hidden--> 
			<input class="npc-tag" name="attr_npc_tag" readonly title="• 크기&#13;초소형 (Tiny), 소형 (Small), 중형 (Medium), 대형 (Large), 거대형 (Huge), 초대형 (Gargantuan)&#13;&#13;• 종류&#13;거인 (Giant), 괴물류 (Monstrosity), 구조물 (Construct), 기괴체 (Aberration), 식물류 (Plant),&#13;악마 (Fiend), 야수 (Beast), 언데드 (Undead), 요정 (Fey), 용족 (Dragon), 원소 (Elemental),&#13;인간형 (Humanoid), 점액류 (Ooze), 천상체 (Celestial)">
			<input class="npc-cr" name="attr_npc_cr" readonly title="• 숙련 보너스&#13;도전 지수 0~4　　　+2&#13;도전 지수 5~8　　　+3&#13;도전 지수 9~12 　　+4&#13;도전 지수 13~16　　+5&#13;도전 지수 17~20　　+6&#13;도전 지수 21~24　　+7&#13;도전 지수 25~28　　+8&#13;도전 지수 29~30　　+9">
		</div>
		<button type="roll" class="npc-init" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=우선권}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_dex_mod}]]}} @{bonus}@{npc_dex_mod}]]}}"></button>
		<div class="npc-section">
			<!-- 능력치 --> 
			<div class="npc-stats">
				<span class="tab-title" style="margin-bottom: 6px;">능력치</span>
				<div>
					<button type="roll" class="dice" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=근력}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_str_mod}]]}} @{bonus}@{npc_str_mod}]]}}">근력</button>
					<input type="number" name="attr_npc_str" accept="STR" value="" placeholder="10"/>
					(<input readonly name="attr_npc_str_mod" value="+0"/>)
					<span style="margin-right: 8px;"/>
					<button type="roll" class="dice" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=근력}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_str_save_mod}]]}} @{bonus}@{npc_str_save_mod}]]}}">근력 내성</button>
					<input type="text" name="attr_npc_str_save_mod" value="" placeholder="+0"/>
				</div>
				<div>
					<button type="roll" class="dice" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=민첩}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_dex_mod}]]}} @{bonus}@{npc_dex_mod}]]}}">민첩</button>
					<input type="number" name="attr_npc_dex" accept="DEX" value="" placeholder="10"/>
					(<input readonly name="attr_npc_dex_mod" value="+0"/>)
					<span style="margin-right: 8px;"/>
					<button type="roll" class="dice" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=민첩}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_dex_save_mod}]]}} @{bonus}@{npc_dex_save_mod}]]}}">민첩 내성</button>
					<input type="text" name="attr_npc_dex_save_mod" value="" placeholder="+0"/>
				</div>
				<div>
					<button type="roll" class="dice" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=건강}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_con_mod}]]}} @{bonus}@{npc_con_mod}]]}}">건강</button>
					<input type="number" name="attr_npc_con" accept="CON" value="" placeholder="10"/>
					(<input readonly name="attr_npc_con_mod" value="+0"/>)
					<span style="margin-right: 8px;"/>
					<button type="roll" class="dice" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=건강}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_con_save_mod}]]}} @{bonus}@{npc_con_save_mod}]]}}">건강 내성</button>
					<input type="text" name="attr_npc_con_save_mod" value="" placeholder="+0"/>
				</div>
				<div>
					<button type="roll" class="dice" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=지능}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_int_mod}]]}} @{bonus}@{npc_int_mod}]]}}">지능</button>
					<input type="number" name="attr_npc_int" accept="INT" value="" placeholder="10"/>
					(<input readonly name="attr_npc_int_mod" value="+0"/>)
					<span style="margin-right: 8px;"/>
					<button type="roll" class="dice" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=지능}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_int_save_mod}]]}} @{bonus}@{npc_int_save_mod}]]}}">지능 내성</button>
					<input type="text" name="attr_npc_int_save_mod" value="" placeholder="+0"/>
				</div>
				<div>
					<button type="roll" class="dice" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=지혜}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_wis_mod}]]}} @{bonus}@{npc_wis_mod}]]}}">지혜</button>
					<input type="number" name="attr_npc_wis" accept="WIS" value="" placeholder="10"/>
					(<input readonly name="attr_npc_wis_mod" value="+0"/>)
					<span style="margin-right: 8px;"/>
					<button type="roll" class="dice" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=지혜}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_wis_save_mod}]]}} @{bonus}@{npc_wis_save_mod}]]}}">지혜 내성</button>
					<input type="text" name="attr_npc_wis_save_mod" value="" placeholder="+0"/>
				</div>
				<div>
					<button type="roll" class="dice" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=매력}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_cha_mod}]]}} @{bonus}@{npc_cha_mod}]]}}">매력</button>
					<input type="number" name="attr_npc_cha" accept="CHA" value="" placeholder="10"/>
					(<input readonly name="attr_npc_cha_mod" value="+0"/>)
					<span style="margin-right: 8px;"/>
					<button type="roll" class="dice" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=매력}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_cha_save_mod}]]}} @{bonus}@{npc_cha_save_mod}]]}}">매력 내성</button>
					<input type="text" name="attr_npc_cha_save_mod" value="" placeholder="+0"/>
				</div>
			</div>
			<!-- 기술-->
			<div class="npc-skills">
				<span class="tab-title" style="margin-bottom: 6px;">기술</span>
				<!--1번 기술-->
				<div class="npc-skill">
					<select name="attr_npc_skill_name_01">
						<option value="감지" selected>감지</option>
						<option value="곡예">곡예</option>
						<option value="공연">공연</option>
						<option value="기만">기만</option>
						<option value="동물 조련">동물 조련</option>
						<option value="비전학">비전학</option>
						<option value="생존">생존</option>
						<option value="설득">설득</option>
						<option value="손속임">손속임</option>
						<option value="수사">수사</option>
						<option value="역사학">역사학</option>
						<option value="운동">운동</option>
						<option value="위협">위협</option>
						<option value="은신">은신</option>
						<option value="의학">의학</option>
						<option value="자연학">자연학</option>
						<option value="종교학">종교학</option>
						<option value="통찰">통찰</option>
					</select>
					<input type="text" name="attr_npc_skill_mod_01" value="" placeholder="+0"/>
					<div class="npc-dice">
						<button type="roll" class="dice" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=@{npc_skill_name_01}}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_skill_mod_01}]]}} @{bonus}@{npc_skill_mod_01}]]}}"></button>
					</div>
				</div>
				<!--2번 기술-->
				<div class="npc-skill">
					<select name="attr_npc_skill_name_02">
						<option value="감지">감지</option>
						<option value="곡예">곡예</option>
						<option value="공연">공연</option>
						<option value="기만" selected>기만</option>
						<option value="동물 조련">동물 조련</option>
						<option value="비전학">비전학</option>
						<option value="생존">생존</option>
						<option value="설득">설득</option>
						<option value="손속임">손속임</option>
						<option value="수사">수사</option>
						<option value="역사학">역사학</option>
						<option value="운동">운동</option>
						<option value="위협">위협</option>
						<option value="은신">은신</option>
						<option value="의학">의학</option>
						<option value="자연학">자연학</option>
						<option value="종교학">종교학</option>
						<option value="통찰">통찰</option>
					</select>
					<input type="text" name="attr_npc_skill_mod_02" value="" placeholder="+0"/>
					<div class="npc-dice">
						<button type="roll" class="dice" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=@{npc_skill_name_02}}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_skill_mod_02}]]}} @{bonus}@{npc_skill_mod_02}]]}}"></button>
					</div>
				</div>
				<!--3번 기술-->
				<div class="npc-skill">
					<select name="attr_npc_skill_name_03">
						<option value="감지">감지</option>
						<option value="곡예">곡예</option>
						<option value="공연">공연</option>
						<option value="기만">기만</option>
						<option value="동물 조련">동물 조련</option>
						<option value="비전학">비전학</option>
						<option value="생존" selected>생존</option>
						<option value="설득">설득</option>
						<option value="손속임">손속임</option>
						<option value="수사">수사</option>
						<option value="역사학">역사학</option>
						<option value="운동">운동</option>
						<option value="위협">위협</option>
						<option value="은신">은신</option>
						<option value="의학">의학</option>
						<option value="자연학">자연학</option>
						<option value="종교학">종교학</option>
						<option value="통찰">통찰</option>
					</select>
					<input type="text" name="attr_npc_skill_mod_03" value="" placeholder="+0"/>
					<div class="npc-dice">
						<button type="roll" class="dice" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=@{npc_skill_name_03}}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_skill_mod_03}]]}} @{bonus}@{npc_skill_mod_03}]]}}"></button>
					</div>
				</div>
				<!--4번 기술-->
				<div class="npc-skill">
					<select name="attr_npc_skill_name_04">
						<option value="감지">감지</option>
						<option value="곡예">곡예</option>
						<option value="공연">공연</option>
						<option value="기만">기만</option>
						<option value="동물 조련">동물 조련</option>
						<option value="비전학">비전학</option>
						<option value="생존">생존</option>
						<option value="설득">설득</option>
						<option value="손속임">손속임</option>
						<option value="수사" selected>수사</option>
						<option value="역사학">역사학</option>
						<option value="운동">운동</option>
						<option value="위협">위협</option>
						<option value="은신">은신</option>
						<option value="의학">의학</option>
						<option value="자연학">자연학</option>
						<option value="종교학">종교학</option>
						<option value="통찰">통찰</option>
					</select>
					<input type="text" name="attr_npc_skill_mod_04" value="" placeholder="+0"/>
					<div class="npc-dice">
						<button type="roll" class="dice" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=@{npc_skill_name_04}}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_skill_mod_04}]]}} @{bonus}@{npc_skill_mod_04}]]}}"></button>
					</div>
				</div>
				<!--5번 기술-->
				<div class="npc-skill">
					<select name="attr_npc_skill_name_05">
						<option value="감지">감지</option>
						<option value="곡예">곡예</option>
						<option value="공연">공연</option>
						<option value="기만">기만</option>
						<option value="동물 조련">동물 조련</option>
						<option value="비전학">비전학</option>
						<option value="생존">생존</option>
						<option value="설득">설득</option>
						<option value="손속임">손속임</option>
						<option value="수사">수사</option>
						<option value="역사학">역사학</option>
						<option value="운동">운동</option>
						<option value="위협">위협</option>
						<option value="은신" selected>은신</option>
						<option value="의학">의학</option>
						<option value="자연학">자연학</option>
						<option value="종교학">종교학</option>
						<option value="통찰">통찰</option>
					</select>
					<input type="text" name="attr_npc_skill_mod_05" value="" placeholder="+0"/>
					<div class="npc-dice">
						<button type="roll" class="dice" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=@{npc_skill_name_05}}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_skill_mod_05}]]}} @{bonus}@{npc_skill_mod_05}]]}}"></button>
					</div>
				</div>
				<!--6번 기술-->
				<div class="npc-skill">
					<select name="attr_npc_skill_name_06">
						<option value="감지">감지</option>
						<option value="곡예">곡예</option>
						<option value="공연">공연</option>
						<option value="기만">기만</option>
						<option value="동물 조련">동물 조련</option>
						<option value="비전학">비전학</option>
						<option value="생존">생존</option>
						<option value="설득">설득</option>
						<option value="손속임">손속임</option>
						<option value="수사">수사</option>
						<option value="역사학">역사학</option>
						<option value="운동">운동</option>
						<option value="위협">위협</option>
						<option value="은신">은신</option>
						<option value="의학">의학</option>
						<option value="자연학">자연학</option>
						<option value="종교학">종교학</option>
						<option value="통찰" selected>통찰</option>
					</select>
					<input type="text" name="attr_npc_skill_mod_06" value="" placeholder="+0"/>
					<div class="npc-dice">
						<button type="roll" class="dice" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=@{npc_skill_name_06}}} {{atk=[[1]]}} {{r1=[[@{d20}@{npc_skill_mod_06}]]}} @{bonus}@{npc_skill_mod_06}]]}}"></button>
					</div>
				</div>
			</div>
			<!--기타 정보-->
			<div style="margin-top: 15px;">
				<div class="npc-data"><span>저항</span><textarea class="npc-resistances" name="attr_npc_resist" accept="Resistances" value=""/></div>
				<div class="npc-data"><span>면역</span><textarea class="npc-immunities" name="attr_npc_immune" accept="Immunities" value=""/></div>
				<div class="npc-data"><span>상태 면역</span><textarea class="npc-immunities" name="attr_npc_immunec" accept="Condition Immunities" value=""/></div>
				<div class="npc-data"><span>언어</span><textarea class="npc-immunities" name="attr_npc_lang" accept="Languages" value=""/></div>
				<textarea class="npc-skill-summary" name="attr_npc_skill_summary" accept="Skills" value=""/>
			</div>
		</div>
		<!-- 특성 -->
		<span class="tab-title">특성 및 능력</span>
		<div class="npc-table">
			<div class="npc-expand title">&nbsp;</div>
			<div class="npc-source title">분류</div>
			<div class="npc-name title">이름</div>
			<div class="npc-limit title">사용 제한</div>
			<div class="npc-attack title">명중</div>
			<div class="npc-range title">사거리</div>
			<div class="npc-damage title">피해</div>
			<div class="npc-dice title">&nbsp;</div>
			<details class="feat-expand">
			<summary></summary>
			<p class="feat-desc"><textarea name="attr_npc_desc_01" value="" placeholder="효과 설명" /></p>
			</details>
			<select class="npc-source" name="attr_npc_source_01">
				<option value selected></option>
				<option value="특성">특성</option>
				<option value="행동">행동</option>
				<option value="추가 행동">추가 행동</option>
				<option value="반응 행동">반응 행동</option>
				<option value="본거지 행동">본거지</option>
				<option value="전설적 행동">전설적</option>
				<option value="기타">기타</option>
			</select>
			<input class="npc-name" type="text" name="attr_npc_name_01" value="" />
			<input class="npc-limit" type="text" name="attr_npc_limit_01" value="" placeholder="1회/일" />
			<input class="npc-attack" type="text" name="attr_npc_atk_01" value="0" />
			<input class="npc-range" type="text" name="attr_npc_range_01" value="" />
			<input class="npc-damage" type="text" name="attr_npc_dmg_01" value="" />
			<div class="npc-dice">
				<button type="roll" class="sheet-dice-icon" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=@{npc_name_01}}} {{timing=@{npc_source_01}}} {{range=@{npc_range_01}}} {{atk=[[@{npc_atk_01}]]}} {{r1=[[@{d20}@{npc_atk_01}]]}} @{bonus}@{npc_atk_01}]]}} {{dmg=@{npc_dmg_01}}} {{desc=@{npc_desc_01}}}"></button>
			</div>
			<!-- 반복되는 NPC 특성 -->
		</div>
		<fieldset class="repeating_npc">
			<details class="feat-expand">
			<summary></summary>
			<p class="feat-desc"><textarea name="attr_npc_desc" value="" placeholder="효과 설명" /></p>
			</details>
			<select class="npc-source" name="attr_npc_source">
				<option value selected></option>
				<option value="특성">특성</option>
				<option value="행동">행동</option>
				<option value="추가 행동">추가 행동</option>
				<option value="반응 행동">반응 행동</option>
				<option value="본거지 행동">본거지</option>
				<option value="전설적 행동">전설적</option>
				<option value="기타">기타</option>
			</select>
			<input class="npc-name" type="text" name="attr_npc_name" value="" />
			<input class="npc-limit" type="text" name="attr_npc_limit" value="" placeholder="1회/일" />
			<input class="npc-attack" type="text" name="attr_npc_atk" value="0" />
			<input class="npc-range" type="text" name="attr_npc_range" value="" />
			<input class="npc-damage" type="text" name="attr_npc_dmg" value="" />
			<div class="npc-dice">
				<button type="roll" class="sheet-dice-icon" value="@{whisper}&{template:npc} {{name=@{character_name}}} {{subject=@{npc_name}}} {{timing=@{npc_source}}} {{range=@{npc_range}}} {{atk=[[@{npc_atk}]]}} {{r1=[[@{d20}@{npc_atk}]]}} @{bonus}@{npc_atk}]]}} {{dmg=@{npc_dmg}}} {{desc=@{npc_desc}}}"></button>
			</div>
		</fieldset>
		<textarea name="attr_npc_memo" class="memo" placeholder="" value="" accept="Content" />
		</div>
	</div>
</div>

<script type="text/worker">
	// Toggle Tabs/
	const buttonlist = ["pc","npc"];
	buttonlist.forEach(button => {
		on(`clicked:${button}`, function() {
			setAttrs({
				tab: button
			});
		});
	});

	const int = score => parseInt(score, 10) || 0;

	const stats = ["str", "dex", "con", "wis", "int", "cha"];
	stats.forEach(stat => {
		on(`change:${stat}`, () => {
			getAttrs([stat], values => {
				const stat_base = int(values[stat]);
				//console.log(stat_base);
				let stat_mod = 0;
				if (stat_base >= 20) stat_mod = "+5";
				else if (stat_base >= 18) stat_mod = "+4";
				else if (stat_base >= 16) stat_mod = "+3";
				else if (stat_base >= 14) stat_mod = "+2";
				else if (stat_base >= 12) stat_mod = "+1";
				else if (stat_base >= 10) stat_mod = "+0";
				else if (stat_base >= 8) stat_mod = "-1";
				else if (stat_base >= 6) stat_mod = "-2";
				else stat_mod = "-3";
				
				setAttrs({
					[`${stat}_mod`]: stat_mod
				});
			});
		});
	});

	const npcstats = ["npc_str", "npc_dex", "npc_con", "npc_wis", "npc_int", "npc_cha"];
	npcstats.forEach(npcstat => {
		on(`change:${npcstat}`, () => {
			getAttrs([npcstat], values => {
				const npcstat_base = int(values[npcstat]);
				//console.log(npcstat_base);
				let npcstat_mod = 0;
				if (npcstat_base >= 28) npcstat_mod = "+9";
				else if (npcstat_base >= 26) npcstat_mod = "+8";
				else if (npcstat_base >= 24) npcstat_mod = "+7";
				else if (npcstat_base >= 22) npcstat_mod = "+6";
				else if (npcstat_base >= 20) npcstat_mod = "+5";
				else if (npcstat_base >= 18) npcstat_mod = "+4";
				else if (npcstat_base >= 16) npcstat_mod = "+3";
				else if (npcstat_base >= 14) npcstat_mod = "+2";
				else if (npcstat_base >= 12) npcstat_mod = "+1";
				else if (npcstat_base >= 10) npcstat_mod = "+0";
				else if (npcstat_base >= 8) npcstat_mod = "-1";
				else if (npcstat_base >= 6) npcstat_mod = "-2";
				else npcstat_mod = "-3";
				
				setAttrs({
					[`${npcstat}_mod`]: npcstat_mod
				});
			});
		});
	});


	on("change:level sheet:opened", function() {
		getAttrs(["level"], function(values) {
			let level = parseInt(values.level)||0;
			let exp = 0;
			if      (level == 1)  exp = "300";
			else if (level == 2)  exp = "900";
			else if (level == 3)  exp = "2,700";
			else if (level == 4)  exp = "6,500";
			else if (level == 5)  exp = "14,000";
			else if (level == 6)  exp = "23,000";
			else if (level == 7)  exp = "34,000";
			else if (level == 8)  exp = "48,000";
			else if (level == 9)  exp = "64,000";
			else if (level == 10)  exp = "85,000";
			else if (level == 11)  exp = "100,000";
			else if (level == 12)  exp = "120,000";
			else if (level == 13)  exp = "140,000";
			else if (level == 14)  exp = "165,000";
			else if (level == 15)  exp = "195,000";
			else if (level == 16)  exp = "225,000";
			else if (level == 17)  exp = "265,000";
			else if (level == 18)  exp = "305,000";
			else if (level == 19) exp = "355,000";
			else exp = "MAX";
			setAttrs({
				"foo_exp": exp
		  });
		});
	});

		on("change:exhaustion sheet:opened", function() {
		getAttrs(["exhaustion"], function(values) {
			let exhaustion = parseInt(values.exhaustion)||0;
			if      (exhaustion == 0)  exh_desc = "탈진 관련 상태 이상 없음";
			else if (exhaustion == 1)  exh_desc = "능력 판정에 불리점";
			else if (exhaustion == 2)  exh_desc = "능력 판정에 불리점, 속도 ½";
			else if (exhaustion == 3)  exh_desc = "능력 · 명중 · 내성 굴림에 불리점, 속도 ½";
			else if (exhaustion == 4)  exh_desc = "능력 · 명중 · 내성에 불리점, 속도 ½, 최대 HP ½";
			else if (exhaustion == 5)  exh_desc = "능력 · 명중 · 내성에 불리점, 속도 ∅, 최대 HP ½";
			else if (exhaustion == 6)  exh_desc = "죽음";
			else exh_desc = "탈진 단계를 입력해주세요 (0~6)";
			setAttrs({
				"foo_exh_desc": exh_desc
		  });
		});
	});

	on("change:spell_type_01 sheet:opened", function() {
	  getAttrs(["spell_type_01"], function(values) {
		let spell_type_01 = parseInt(values.spell_type_01) || 0;
		let spl_atk, spl_dc;

		switch (spell_type_01) {
		  case 0:
			spl_atk = "@{spell_atk}";
			spl_dc = "";
			break;
		  case 1:
			spl_atk = "0";
			spl_dc = "DC 근력 @{spell_dc}";
			break;
		  case 2:
			spl_atk = "0";
			spl_dc = "DC 민첩 @{spell_dc}";
			break;
		  case 3:
			spl_atk = "0";
			spl_dc = "DC 건강 @{spell_dc}";
			break;
		  case 4:
			spl_atk = "0";
			spl_dc = "DC 지능 @{spell_dc}";
			break;
		  case 5:
			spl_atk = "0";
			spl_dc = "DC 지혜 @{spell_dc}";
			break;
		  case 6:
			spl_atk = "0";
			spl_dc = "DC 매력 @{spell_dc}";
			break;
		  case 7:
			spl_atk = "0";
			spl_dc = "";
			break;
		  default:
			spl_atk = "@{spell_atk}";
			spl_dc = "";
			break;
		}

		setAttrs({
		  "foo_spl_atk": spl_atk,
		  "foo_spl_dc": spl_dc
		});
	  });
	});


	on('sheet:opened change:repeating_spell:spell_type', function() {
		getSectionIDs('spell', function(idarray) {
		  // first get the attribute names for all rows in put in one array
		  const fieldnames = [];
		  idarray.forEach(id => fieldnames.push(`repeating_spell_${id}_spell_type`));
		  
		  getAttrs(fieldnames, values => {
			// create a variable to hold all the attribute values youre going to create.
			const attr = {};
			// now loop through the rows again
			idarray.forEach(id => {
			  let row = 'repeating_spell_' + id;
			  let sptype = values[`${row}_spell_type`] || 0;
			  if (sptype == 0) {
				attr[`${row}_rep_atk`] = "@{spell_atk}";
				attr[`${row}_rep_dc`] = "";
			  }
			  else if (sptype == 1) {
				attr[`${row}_rep_atk`] = "0";
				attr[`${row}_rep_dc`] = "DC 근력 @{spell_dc}";
			  }
			  else if (sptype == 2) {
				attr[`${row}_rep_atk`] = "0";
				attr[`${row}_rep_dc`] = "DC 민첩 @{spell_dc}";
			  }
			  else if (sptype == 3) {
				attr[`${row}_rep_atk`] = "0";
				attr[`${row}_rep_dc`] = "DC 건강 @{spell_dc}";
			  }
			  else if (sptype == 4) {
				attr[`${row}_rep_atk`] = "0";
				attr[`${row}_rep_dc`] = "DC 지능 @{spell_dc}";
			  }
			  else if (sptype == 5) {
				attr[`${row}_rep_atk`] = "0";
				attr[`${row}_rep_dc`] = "DC 지혜 @{spell_dc}";
			  }
			  else if (sptype == 6) {
				attr[`${row}_rep_atk`] = "0";
				attr[`${row}_rep_dc`] = "DC 매력 @{spell_dc}";
			  }
			  else if (sptype == 7) {
				attr[`${row}_rep_atk`] = "0";
				attr[`${row}_rep_dc`] = "";
			  }
			  else {
				attr[`${row}_rep_atk`] = "@{spell_atk}";
				attr[`${row}_rep_dc`] = "";
			  }
			});
			console.log(values,attr)
			setAttrs(attr);
		  });
		});
	  });

	on('change:npc_size change:npc_type change:npc_alignment change:npc_challenge sheet:opened', function() {
		getAttrs(["npc_size", "npc_type", "npc_alignment", "npc_challenge"], function(values) {
			let size = values.npc_size || "";
			let type = values.npc_type || "";
			let align = values.npc_alignment || "";
			let chal = values.npc_challenge || "";

			// Combine size, type, and alignment into a single string
			let tag = size + " " + type + ", " + align;
			let cr = "도전 지수 " + chal;

			// Update npc_tag attribute with the combined string
			setAttrs({
				npc_tag: tag,
				npc_cr : cr
			});
		});
	});
</script>

<rolltemplate class="sheet-rolltemplate-custom">
	<div class="sheet-rtw">
		<div class="sheet-rth">
			{{#name}}
			<span>{{name}}</span>
			{{/name}}
			{{#subject}}
			<span>{{subject}}{{#advantage}}・이점{{/advantage}}{{#disadvantage}}・불리점{{/disadvantage}}</span>
			{{#timing}}<span class="tag hl">{{timing}}</span>{{/timing}}
			{{#limit}}<span class="tag">{{limit}}</span>{{/limit}}
			{{#range}}<span class="tag">사거리 {{range}}</span>{{/range}}
			{{#dc}}<span class="tag">{{dc}}</span>{{/dc}}
			{{#duration}}<span class="tag">{{duration}}</span>{{/duration}}
			{{#vsm}}<span class="tag">구성 {{vsm}}</span>{{/vsm}}
			{{/subject}}
		</div>
		<div class="sheet-rtb">
			<div class="sheet-result">
				<!-- 일반 판정-->
				{{#normal}}
				{{#^rollTotal() atk 0}}{{r1}}{{/^rollTotal() atk 0}}
				{{/normal}}
				{{#advantage}}
				{{#^rollTotal() atk 0}}
				{{#rollLess() r1 r2}}
				<span class="sheet-grey">{{r1}}{{#global}} + {{global}}{{/global}}</span>
				<span>✦</span>
				<span>{{r2}}{{#global}} + {{global}}{{/global}}</span>
				{{/rollLess() r1 r2}}
				{{/^rollTotal() atk 0}}
				{{#^rollTotal() atk 0}}
				{{#rollTotal() r1 r2}}
				<span>{{r1}}{{#global}} + {{global}}{{/global}}</span>
				<span>✦</span>
				<span>{{r2}}{{#global}} + {{global}}{{/global}}</span>
				{{/rollTotal() r1 r2}}
				{{/^rollTotal() atk 0}}
				{{#^rollTotal() atk 0}}
				{{#rollGreater() r1 r2}}
				<span>{{r1}}{{#global}} + {{global}}{{/global}}</span>
				<span>✦</span>
				<span class="sheet-grey">{{r2}}{{#global}} + {{global}}{{/global}}</span>
				{{/rollGreater() r1 r2}}
				{{/^rollTotal() atk 0}}
				{{/advantage}}
				{{#disadvantage}}
				{{#^rollTotal() atk 0}}
				{{#rollLess() r1 r2}}
				<span>{{r1}}{{#global}} + {{global}}{{/global}}</span>
				<span>✦</span>
				<span class="sheet-grey">{{r2}}{{#global}} + {{global}}{{/global}}</span>
				{{/rollLess() r1 r2}}
				{{/^rollTotal() atk 0}}
				{{#^rollTotal() atk 0}}
				{{#rollTotal() r1 r2}}
				<span>{{r1}}{{#global}} + {{global}}{{/global}}</span>
				<span>✦</span>
				<span>{{r2}}{{#global}} + {{global}}{{/global}}</span>
				{{/rollTotal() r1 r2}}
				{{/^rollTotal() atk 0}}
				{{#^rollTotal() atk 0}}
				{{#rollGreater() r1 r2}}
				<span class="sheet-grey">{{r1}}{{#global}} + {{global}}{{/global}}</span>
				<span>✦</span>
				<span>{{r2}}{{#global}} + {{global}}{{/global}}</span>
				{{/rollGreater() r1 r2}}
				{{/^rollTotal() atk 0}}
				{{/disadvantage}}
			</div>
			{{#dmg}}
			<div class="sheet-dmg">피해: {{dmg}}</div>
			{{/dmg}}
			{{#desc}}
			<div class="desc">{{desc}}</div>
			{{/desc}}
		</div>
	</div>
</rolltemplate>

<rolltemplate class="sheet-rolltemplate-npc">
	<div class="sheet-rtw">
		<div class="sheet-rth">
			{{#name}}
				<span>{{name}}</span>
			{{/name}}
			{{#subject}}
				<span>{{subject}}{{#advantage}}・이점{{/advantage}}{{#disadvantage}}・불리점{{/disadvantage}}</span>
				{{#timing}}<span class="sheet-tag sheet-hl">{{timing}}</span>{{/timing}}
				{{#limit}}<span class="sheet-tag">{{limit}}</span>{{/limit}}
				{{#range}}<span class="sheet-tag">사거리 {{range}}</span>{{/range}}
				{{#dc}}<span class="sheet-tag">DC {{dc}}</span>{{/dc}}
			{{/subject}}
		</div>
		<div class="sheet-rtb">
			<!-- 일반 판정-->
			{{#normal}}
				{{#^rollTotal() atk 0}}{{r1}}{{/^rollTotal() atk 0}}
			{{/normal}}
			{{#advantage}}
				{{#^rollTotal() atk 0}}
				{{#rollLess() r1 r2}}
					<span class="sheet-grey">{{r1}}{{#global}} + {{global}}{{/global}}</span>
					<span>✦</span>
					<span>{{r2}}{{#global}} + {{global}}{{/global}}</span>
				{{/rollLess() r1 r2}}
				{{/^rollTotal() atk 0}}
				{{#^rollTotal() atk 0}}
				{{#rollTotal() r1 r2}}
					<span>{{r1}}{{#global}} + {{global}}{{/global}}</span>
					<span>✦</span>
					<span>{{r2}}{{#global}} + {{global}}{{/global}}</span>
				{{/rollTotal() r1 r2}}
				{{/^rollTotal() atk 0}}
				{{#^rollTotal() atk 0}}
				{{#rollGreater() r1 r2}}
					<span>{{r1}}{{#global}} + {{global}}{{/global}}</span>
					<span>✦</span>
					<span class="sheet-grey">{{r2}}{{#global}} + {{global}}{{/global}}</span>
				{{/rollGreater() r1 r2}}
				{{/^rollTotal() atk 0}}
			{{/advantage}}
			{{#disadvantage}}
				{{#^rollTotal() atk 0}}
				{{#rollLess() r1 r2}}
					<span>{{r1}}{{#global}} + {{global}}{{/global}}</span>
					<span>✦</span>
					<span class="sheet-grey">{{r2}}{{#global}} + {{global}}{{/global}}</span>
				{{/rollLess() r1 r2}}
				{{/^rollTotal() atk 0}}
				{{#^rollTotal() atk 0}}
				{{#rollTotal() r1 r2}}
					<span>{{r1}}{{#global}} + {{global}}{{/global}}</span>
					<span>✦</span>
					<span>{{r2}}{{#global}} + {{global}}{{/global}}</span>
				{{/rollTotal() r1 r2}}
				{{/^rollTotal() atk 0}}
				{{#^rollTotal() atk 0}}
				{{#rollGreater() r1 r2}}
					<span class="sheet-grey">{{r1}}{{#global}} + {{global}}{{/global}}</span>
					<span>✦</span>
					<span>{{r2}}{{#global}} + {{global}}{{/global}}</span>
				{{/rollGreater() r1 r2}}
				{{/^rollTotal() atk 0}}
			{{/disadvantage}}
			{{#dmg}}
				<div class="sheet-dmg">피해: {{dmg}}</div>
			{{/dmg}}
			{{#desc}}
				<div class="desc">{{desc}}</div>
			{{/desc}}
		</div>
	</div>
</rolltemplate>
</body>
</html>
