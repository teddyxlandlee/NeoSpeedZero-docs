<!-- _sidebar.md -->

<!--* [Preface 前言](README.md)-->
* 中文
    * [前言](zh/README)
    * [速通目标](zh/goal)
    * [难度设置](zh/difficulty)
* English
    * [Preface](en/README)
    * [Speedrun Goals](en/goal)
    * [Difficulty Settings](en/difficulty)

<div id='x-version-string' style='color:dimgray;font-size:.8em'></div>

<script>
(async function () {
  'use strict';
  const version = await fetch('/version.txt');
  let versionString = await version.text();
  versionString = versionString.trim();

  document.getElementById('x-version-string').innerText = `Last Updated: ${versionString}`;
})();
</script>
<!-- ↑ User defined: Last Updated -->
