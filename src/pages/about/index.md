---
templateKey: about-page
title: 海伯利安诗篇
---
# 简介
《海伯利安诗篇》（Hyperion Cantos）首部作品《海伯利安》于1989年出版，这部小说深受英国诗人乔叟《坎特伯雷故事集》与意大利作家乔万尼·薄伽丘《十日谈》的影响，“海伯利安”这个名称则来自诗人约翰·济慈。《海伯利安诗篇》背景设定在公元28世纪，当人类已能够离开地球，以移动科技传送门（farcasters）在银河系建立巨大的世界网（WorldWeb），而海伯利安行星是故事核心。《海伯利安》获得广泛赞誉，也荣获雨果奖、轨迹奖，并入围亚瑟·克拉克奖与英国科幻小说协会奖。

<script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.1.5/jszip.min.js"></script>
 <script src="https://cdn.jsdelivr.net/npm/epubjs/dist/epub.min.js"></script>
 <style type="text/css">
   .arrow {
      margin: 14px;
      display: inline-block;
      text-align: center;
      text-decoration: none;
      color: #ccc;
    }
    .arrow:hover {
      color: #777;
    }
    .arrow:active {
      color: #000;
    }
    #prev {
      float: left;
    }
    #next {
      float: right;
    }
    #toc {
      display: block;
      margin: 10px auto;
      height: 30px;
    }
  </style>
 <div id="navigation">
 <select id="toc"></select>
 </div>
   <div id="main">
    <div id="pagination">
    <a id="prev" href="#prev" class="arrow">...</a>
    </div>
    <div id="viewer" class="scrolled"></div>
    <a id="next" href="#next" class="arrow">...</a>
  </div>

<script>
    var book = ePub("https://res.cloudinary.com/hppcggqdk/raw/upload/v1556030851/blog-docu/books/Dan%20Simmons/%E6%B5%B7%E4%BC%AF%E5%88%A9%E5%AE%89%E5%9B%9B%E9%83%A8%E6%9B%B2_%E9%87%8D%E6%8E%92%E5%BC%B9%E6%B3%A8.epub");
     var rendition = book.renderTo("viewer", {
      flow: "scrolled-doc",
      width: "100%"
    });
    var displayed = rendition.display();
    var next = document.getElementById("next");
    next.addEventListener("click", function(e){
      rendition.next();
      e.preventDefault();
    }, false);
    var prev = document.getElementById("prev");
    prev.addEventListener("click", function(e){
      rendition.prev();
      e.preventDefault();
    }, false);
    rendition.on("relocated", function(location){
      console.log(location);
    });
    rendition.on("rendered", function(section){
      var nextSection = section.next();
      var prevSection = section.prev();
      if(nextSection) {
        nextNav = book.navigation.get(nextSection.href);
        if(nextNav) {
          nextLabel = nextNav.label;
        } else {
          nextLabel = "next";
        }
        next.textContent = nextLabel + " »";
      } else {
        next.textContent = "";
      }
      if(prevSection) {
        prevNav = book.navigation.get(prevSection.href);
        if(prevNav) {
          prevLabel = prevNav.label;
        } else {
          prevLabel = "previous";
        }
        prev.textContent = "« " + prevLabel;
      } else {
        prev.textContent = "";
      }
    });

        book.loaded.navigation.then(function(toc){
      var $select = document.getElementById("toc"),
          docfrag = document.createDocumentFragment();
      toc.forEach(function(chapter) {
        var option = document.createElement("option");
        option.textContent = chapter.label;
        option.ref = chapter.href;
        docfrag.appendChild(option);
      });
      $select.appendChild(docfrag);
      $select.onchange = function(){
          var index = $select.selectedIndex,
              url = $select.options[index].ref;
          rendition.display(url);
          return false;
      };
      book.opened.then(function(){
        display(currentSectionIndex);
      });
      $next.addEventListener("click", function(){
        var displayed = display(currentSectionIndex+1);
        if(displayed) currentSectionIndex++;
      }, false);
      $prev.addEventListener("click", function(){
        var displayed = display(currentSectionIndex-1);
        if(displayed) currentSectionIndex--;
      }, false);
      function display(item){
        var section = book.spine.get(item);
        if(section) {
          currentSection = section;
          section.render().then(function(html){
            $viewer.innerHTML = html;
          });
        }
        return section;
      }
    });
 
   </script>
