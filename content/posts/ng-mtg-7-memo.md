---
title: '[WIP] ng-mtg#7 参加 メモ'
date: 2016-05-18T10:25:03+09:00
draft: true
---

とりあえず参加時のメモをそのまま
加筆修正は徐々に

勉強会 概要
https://angularjs-jp.doorkeeper.jp/events/42684

ng-mtg#7 Angular勉強会
2016-05-17（火）19:30 - 21:30


Reference for the Angular 2 ecosystem (@albatrosary)
#ng-conf で紹介された周辺ツールの紹介 & デモ

angular-cli
http://albatrosary.hateblo.jp/entry/2016/03/18/101637

 * ember 由来
 * yeoman みたいなもの

$ npm install -g angular-cli
$ ng new SampleProject
$ ng serve


Angular Universal
https://github.com/angular/universal

サーバサイドレンダリング

Angular Mobile Toolkit
ng new aaa --mobile


--mobile manifest file を作ってくれる

Angular Augury
https://github.com/rangle/augury
https://augury.angular.io/
http://augury.rangle.io/

Chrome extension

Angular Material 2
https://material.angular.io/

Angular Firebase 2
Require TypeScript

Angular 2 Style Guide
引き続き、これに沿って

Angular Referrences
Angular Contributions
http://www.ng-contrib.org/

Codelyzer
Lint

https://github.com/mgechev/codelyzer

> A set of tslint rules for static code analysis of Angular 2 TypeScript projects.


$ npm install --save-dev codelyzer


Pick up #ngconf (@can.i.do.web)
pick up ng-conf [//www.slideshare.net/canidoweb/pick-up-ngconf] from Kenichi
Kanai [//www.slideshare.net/canidoweb] Framework to Platform

様々なツールを提供 (Angular Angury etc)

Angular Universal
こっちの方が簡単 #ﾄﾉｺﾄ

https://github.com/angular/universal-starter

Animation
Web-Animations Polyfill

Working Draft
https://www.w3.org/TR/web-animations-1/

デモ

:memo: 結構複雑なアニメーションもできる
:memo: どう使うかは... やりすぎ注意

Hirez.io

Youtube : ng-conf 2016

Decorator (@yoshiko-pg)
http://yoshiko-pg.github.io/slides/20160517-ngmtg/#1

スライドをご覧ください

Angular2-rc.1 Unit Testing Overview (@mitsuruog)
Angular2 rc.1 unit testing overview
[//www.slideshare.net/mitsuruogawa33/angular2-rc1-unit-testing-overview] from
Mitsuru Ogawa [//www.slideshare.net/mitsuruogawa33]  * Angular1のテスト資産をどう生かすか

Angular2 テストの基本
TypeScript
Jasmine
SystemJS -> mobule loader
Karma

zone.js

今のうちに Controller のロジックを Service に切り出していって
ピュアなロジックをまとめておくと移植もテストもしやすくなる

-> ControllerHelper
