---
# You can also start simply with 'default'
theme: default
# layout: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
# background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: D3 - 事件、互動、以及動畫
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: true
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
---

# D3 - 事件、互動以及動畫

Laura

<!-- <div @click="$slidev.nav.next" class="mt-12 py-1" hover:bg="white op-10">
  GOGO <carbon:arrow-right />
</div> -->

<style>

h1 {
  font-weight: bold;
  letter-spacing: 1.25px;
  font-size: 2.25rem !important;
  margin-bottom: 0.5rem;
}
p {
  color: #000;
  font-weight: bold;
}
</style>
<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
layout: two-cols
layoutClass: gap-16
---

# 

**大綱**

::right::

<Toc text-sm minDepth="1" maxDepth="2" /> 


---
transition: fade
mdc: true
---

## 事件 ( DOM 事件 )
: 基本上任一 **DOM元素** 皆可接收事件並呼叫相對應的處理程式

`4-1. 一些重要的使用者產生事件狀態`

| 函式                                        | 說明                                             |
| ------------------------------------------- | ----------------------------------------------- |
| <kbd>click</kbd>                            | 在元素上的任何滑鼠按鈕點擊                         |
| <kbd>mousemove</kbd>                        | 滑鼠的游標滑過元素                                |
| <kbd>mousedown</kbd> <kbd>mouseup</kbd>     | 滑鼠按鈕在元素上被壓下或釋放                       |
| <kbd>mouseenter</kbd> <kbd>mouseleave</kbd> | 滑鼠游標被移入或是一出一個元素                     |
| <kbd>mouseover</kbd> <kbd>mouseout</kbd>    | 滑鼠游標被移入或是一出一個元素，或是任一個它的子項目 |
| <kbd>keydown</kbd> <kbd>keyup</kbd>         | 任一鍵盤上的案件被按下或是釋放                     |

<style>
p {
  color:rgb(255, 255, 255);
}
th {
  background-color:rgb(42, 42, 42);
  font-weight: bold;
  padding: 0.5rem;
}
tr {
  border-color: rgb(255 255 255 / 30%);
}
td {
  color: rgb(42, 42, 42);
  font-size: 0.85rem;
  font-weight: bold !important;
}
</style>

---
transition: slide-up
mdc: true
---

## 事件 ( D3 )
: 基本上任一 **DOM元素** 皆可接收事件並呼叫相對應的處理程式

`4-2. 一些和事件處理相關的方法、變數以及函式 ( sel 是 Selection 物件 )`

| 函式                                        | 說明                                                 |
| ------------------------------------------- | --------------------------------------------------- |
| <kbd>sel.on(types, callback)</kbd>          | 對在 Selection 中的每一個元素新增或移除一個 callback。 <br> <ul><li>types 參數必須是字串</li> <li>若有指定 callback，它會被註冊為事件；已經註冊的事件則會被移除。</li> <li> 若 callback 參數是 null，所有已存在的處理程式均會被移除</li> <li>若沒有設定 callback 參數，則會回傳目前設定的處理程式</li></ul>                       |
| <kbd>d3.event</kbd>                         | 如果有的話，會包含作為 DOM Event 物件目前的事件。       |
| <kbd>d3.mouse(parent)</kbd>                 | 傳回一個二元素陣列，此陣列包含相對於 parent 的滑鼠座標。 |
| <kbd>sel.dispatch(type)</kbd>               | 指派指定型態的自訂事件到目前 selection 中的所有元素。    |
<style>
p {
  color:rgb(255, 255, 255);
}
th {
  background-color:rgb(42, 42, 42);
  font-weight: bold;
  padding: 0.5rem;
}
tr {
  border-color: rgb(255 255 255 / 30%);
}
td {
  color: rgb(42, 42, 42);
  font-size: 0.85rem;
  font-weight: bold !important;
}
</style>

<!-- D3 使用 sel.on 去註冊事件，其中 type 參數必須是一個事件型態的字串 (例如 'click' )，任何 DOM 事件型態都可以使用。

如果一個事件處理器已經被 on() 註冊到某一個事件型態，則會在新的事件處理器註冊之前先把舊的移除。

為了明確地移除特定事件型態的事件處理器，可以在第二個參數中提供一個null值  -->

---
transition: slide-down
---
## 使用滑鼠探索圖形

 <!-- TwoSlash enables TypeScript hover information
 and errors in markdown code blocks
 More at https://shiki.style/packages/twoslash -->
 
```js {all|2|3|5|6|all}

  function coordsPixels(selector) {
    const txt = d3.select(selector).append('text');
    const svg = d3.select(selector).attr('cursor', 'crosshair')
      .on('mousemove', function(event) {
        const pt = d3.pointer(event, svg.node());
        txt.attr('x', 18 + pt[0]).attr('y', 6 + pt[1])
          .text('' + pt[0] + ',' + pt[1])
      })
  }

```
<svg width="100%" height="250" id="svgCanvas"></svg>

<script setup>
  import * as d3 from 'd3';
  import { onMounted } from 'vue';
  
  function coordsPixels(selector) {
    const txt = d3.select(selector).append('text');
    const svg = d3.select(selector).style('cursor', 'crosshair')
      .on('mousemove', function(event) {
        const pt = d3.pointer(event, svg.node());
        txt.attr('x', 18 + pt[0]).attr('y', 6 + pt[1])
          .text(''+ pt[0] + ',' + pt[1])
      })
  }

  onMounted(() => {
    coordsPixels('#svgCanvas')
  });

</script> 

<style>
h1 {
  font-weight: bold;
  letter-spacing: 1.5px;
  font-size: 1.75rem;
}
svg {
  border: 1px solid #fff;
  border-radius: 10px;
  margin-top: 1rem;
}
</style> 

<!-- 
[click] 1. 建立一個 ```<text>``` 用來顯示座標。記得把它宣告在事件處理程式外面，不然會在每次滑鼠移動時都建立新的 ```<text>```

[click] 2. 當滑鼠滑過 ```<svg>``` 時改變滑鼠游標的形狀。

[click] 3. 取得滑鼠的座標，相對於 ```<svg>```元素的左上角。

[click] 4. 更新之前建立的 text 元素
-->


---


## 案例研究 - 同步強調效果
: 在進行多變量資料集中常見的問題，如何連結兩個不同的視圖或是映射，以下是簡化過的例子。
<div style="display: flex; align-items: center; justify-content: center">
  <svg id="brush1" width="380" height="300" style="background:white"></svg>
  <svg id="brush2" width="380" height="300" style="background:white"></svg>
</div>

<script setup>
  import * as d3 from 'd3';
  import { onMounted } from 'vue';
  
  function makeBrush() {
    d3.csv('/dense.csv').then(function(data){
      const svg1 = d3.select('#brush1');
      const svg2 = d3.select('#brush2');
      const sc1 = d3.scaleLinear().domain([0, 10, 50])
            .range(['lime', 'yellow', 'red']);
      const sc2 = d3.scaleLinear().domain([0, 10, 50])
            .range(['lime', 'yellow', 'blue']);
      const cs1 = drawCircles(svg1, data, d=> +d['A'], d=> +d['B'], sc1);
      const cs2 = drawCircles(svg2, data, d=> +d['A'], d=> +d['B'], sc2);

      svg1.call(installHandlers2, data, cs1, cs2, sc1, sc2);
    })
  }

  function drawCircles(svg, data, accX, accY, sc) {
    const color = sc(Infinity);
    return svg.selectAll('circle').data(data).enter()
           .append('circle')
           .attr('r', 5).attr('cx', accX).attr('cy', accY)
           .attr('fill', color).attr('fill-opacity', 0.4);
  }

  function installHandlers2( svg, data, cs1, cs2, sc1, sc2 ) {
    const cursor = svg.append( "circle" ).attr( "r", 50 )          
        .attr( "fill", "none" ).attr( "stroke", "black" )
        .attr( "stroke-width", 10 ).attr( "stroke-opacity", 0.1 )
        .attr( "visibility", "hidden" );                         
    
    const hotzone = svg.append( "rect" ).attr( "cursor", "none" )  
        .attr( "x", 50 ).attr( "y", 50 )
        .attr( "width", 200 ).attr( "height", 200 )
        .attr( "visibility", "hidden" )                       
        .attr( "pointer-events", "all" )              
    
        .on( "mouseenter", function() {                      
            cursor.attr( "visibility", "visible" ); } )                
        
        .on( "mousemove", function(event) {                      
            const pt = d3.pointer( event,svg.node() );
            cursor.attr( "cx", pt[0] ).attr( "cy", pt[1] );

            cs1.attr( "fill", function( d, i ) {
                const dx = pt[0] - d3.select( this ).attr( "cx" );
                const dy = pt[1] - d3.select( this ).attr( "cy" );
                const r = Math.hypot( dx, dy );

                data[i]["r"] = r;
                return sc1(r); } );
            
            cs2.attr( "fill", (d,i) => sc2( data[i]["r"] ) ); } )

        .on( "mouseleave", function() {                                 
            cursor.attr( "visibility", "hidden" );
            cs1.attr( "fill", sc1(Infinity) );
            cs2.attr( "fill", sc2(Infinity) ); } )
  }

  makeBrush()

</script> 

<style>
svg {
  margin: 2rem 1rem;
  border-radius: 10px;
}
</style>

<!--
Notes can also sync with clicks

[click] This will be highlighted after the first click

[click] Highlighted with `count = ref(0)`

[click:3] Last click (skip two clicks)
-->

---
# layout: two-cols
# layoutClass: gap-12
---

## 同步效果 code(1)


```js {*|2|3-4|*}
  function makeBrush() {
    d3.csv('/dense.csv').then(function(data){
      const svg1 = d3.select('#brush1');
      const svg2 = d3.select('#brush2');
      const sc1 = d3.scaleLinear().domain([0, 10, 50])
            .range(['lime', 'yellow', 'red']);
      const sc2 = d3.scaleLinear().domain([0, 10, 50])
            .range(['lime', 'yellow', 'blue']);
      const cs1 = drawCircles(svg1, data, d=> +d['A'], d=> +d['B'], sc1);
      const cs2 = drawCircles(svg2, data, d=> +d['A'], d=> +d['B'], sc2);

      svg1.call(installHandlers, data, cs1, cs2, sc1, sc2);
    })
  }

  function drawCircles(svg, data, accX, accY, sc) {
    const color = sc(Infinity);
    return svg.selectAll('circle').data(data).enter()
           .append('circle')
           .attr('r', 5).attr('cx', accX).attr('cy', accY)
           .attr('fill', color).attr('fill-opacity', 0.4);
  }
```

<!-- 
[click] 1. 載入 .csv 並設定在資料準備好時即可呼叫的 callback 函式。

[click] 2. 選取兩個```svg```
 -->

---

## 同步效果 code(2)

```js {*|2|5|6|10|11|12-18|22-25|*}
function installHandlers2( svg, data, cs1, cs2, sc1, sc2 ) {
  const cursor = svg.append( "circle" ).attr( "r", 50 )          
    .attr( "fill", "none" ).attr( "stroke", "black" )
    .attr( "stroke-width", 10 ).attr( "stroke-opacity", 0.1 )
    .attr( "visibility", "hidden" );          
  const hotzone = svg.append( "rect" ).attr( "cursor", "none" )  
    .attr( "x", 50 ).attr( "y", 50 )
    .attr( "width", 200 ).attr( "height", 200 )
    .attr( "visibility", "hidden" )                       
    .attr( "pointer-events", "all" )              
    .on( "mouseenter", function() { cursor.attr( "visibility", "visible" ) } )            
    .on( "mousemove", function( event ) {                      
      const pt = d3.pointer( event,svg.node() );
      cursor.attr( "cx", pt[0] ).attr( "cy", pt[1] );
      cs1.attr( "fill", function( d, i ) {
        const dx = pt[0] - d3.select( this ).attr( "cx" );
        const dy = pt[1] - d3.select( this ).attr( "cy" );
        const r = Math.hypot( dx, dy );
        data[i]["r"] = r;
      return sc1(r)});  
      cs2.attr( "fill", (d,i) => sc2( data[i]["r"] ) ); } )
         .on( "mouseleave", function() {                                 
         cursor.attr( "visibility", "hidden" );
         cs1.attr( "fill", sc1(Infinity) );
         cs2.attr( "fill", sc2(Infinity) ); 
      })
}
```

<!-- 
[click] 1. 實際滑鼠游標被隱藏起來，取而代之的是一個比較大的圓形。

[click] 2. 只有在滑鼠進去熱點區域的時候才會顯示。

[click] 3. 定義一個矩形的工作區域。事件驅動器被註冊在這個矩形上，等於只有當滑鼠游標在矩形裡，才會被呼叫。

[click] 4. 這個矩形在視圖上是看不到的，但當 DOM 元素的 visibility 屬性被設為 hidden 時，就不會接收滑鼠指標的事件，所以這邊要設定 pointer-events 屬性。

[click] 5. 當滑鼠進入熱點區域的時候，滑鼠會變成不透明的圓形來顯示。

[click] 6. 當滑鼠移動時，對於左邊面板的每一個點，計算它和滑鼠游標的距離。

[click] 7. 當滑鼠離開左側面板時，恢復資料點原來的顏色。
-->

---
class: px-20
---

## D3 拖曳行為元件

: D3 包含一系列預先定義的行為元件，像是只需要透過 **綁定** 和 **組織需要的動作** 就可以做到 <br>`drag-and-drop (拖放)`

<svg id="dragdrop" width="100%" height="200" style="">
  <circle cx="100" cy="100" r="20" fill="rgb(162, 255, 49)" />
  <circle cx="400" cy="100" r="20" fill="rgb(3, 255, 234)" />
  <circle cx="700" cy="100" r="20" fill="rgb(252, 28, 192)" />
</svg>

<script setup>
import * as d3 from 'd3';
import { onMounted } from 'vue';

function makeDragDrop() {
  let widget = undefined, color = undefined;
  const drag = d3.drag()                                    
    .on( "start", function() {                              
        color = d3.select( this ).attr( "fill" );
        widget = d3.select( this ).attr( "fill", "black" );
    } )
    .on( "drag", function(event) {                               
        const pt = d3.pointer( event, d3.select( this ).node() );
        widget.attr( "cx", pt[0] ).attr( "cy", pt[1] );
    } )
    .on( "end", function() {                           
        widget.attr( "fill", color );
        widget = undefined;
    } );

    drag( d3.select( "#dragdrop" ).selectAll( "circle" ) ); 
}
onMounted(() => {
  makeDragDrop()
})
</script>

<style>
svg {
  margin-top: 3rem;
  border-radius: 1rem;
  border: 1px solid #fff;
}
</style>

---

## Drag-and-drop code

```js {*|4|5-8|10-13|15-18|20|*}

  function makeDragDrop() {
    let widget = undefined, color = undefined;

    const drag = d3.drag() 
      .on( "start", function() {                              
        color = d3.select( this ).attr( "fill" );
        widget = d3.select( this ).attr( "fill", "black" );
      })

      .on( "drag", function(event) {                               
        const pt = d3.pointer( event, d3.select( this ).node() );
        widget.attr( "cx", pt[0] ).attr( "cy", pt[1] );
      })

      .on( "end", function() {                           
        widget.attr( "fill", color );
        widget = undefined;
      });

      drag( d3.select( "#dragdrop" ).selectAll( "circle" ) ); 
  }

```

<!-- 
[click] 1. 使用 d3.drag() 建立一個 drag 函式物件

[click] 2. 使用 on 去建立事件，start 這行處理儲存被選擇到的圓形當前顏色。然後變更此圓形的顏色，並把此圓形本身儲存到 widget 這個變數。

[click] 3. drag 事件擷取目前的滑鼠座標，然後把選取到的圓形移動到這個位置。

[click] 4. end 事件回復目前圓形的顏色，並清除作用中的 widget。

[click] 5. 最後呼叫 drag 元件的方法，提供一個包含 circle 的 selection，讓這個圓形安裝已經配置好的事件處理程式到 selection 上。

6. 這裡 on 綁得並不是標準的 DOM 事件，而是 D3 的虛擬事件。D3 的 drag 行為結合了滑鼠和觸控面板事件處理。start 虛擬事件對應到 mousedown 或是 touchstart 事件，drag 和 end 也是類似這種情況。

-->


---

## 建立以及配置 Transition

<div style="display: flex;justify-content:center;margin-top: 2rem">
  <svg id="stagger" width="550" height="300" style="background:black; border-radius: 10px" />
</div>

<script setup lang="ts">
import * as d3 from 'd3';
import { onMounted } from 'vue';
function makeStagger() {
    let ds1 = [ 2, 1, 3, 5, 7, 8, 9, 9, 9, 8, 7, 5, 3, 1, 2 ];   
    let ds2 = [ 8, 9, 8, 7, 5, 3, 2, 1, 2, 3, 5, 7, 8, 9, 8 ];
    let n = ds1.length;
    let mx = d3.max( d3.merge( [ds1, ds2] ) ); 
    
    const svg = d3.select( "#stagger" );

    const scX = d3.scaleLinear().domain( [0,n] ).range( [50,540] );
    const scY = d3.scaleLinear().domain( [0,mx] ).range( [250,50] );

    svg.selectAll( "line" ).data( ds1 ).enter().append( "line" )  
      .attr( "stroke", "rgb(252, 28, 192)" ).attr( "stroke-width", 20 )
      .attr( "x1", (d,i)=>scX(i) ).attr( "y1", scY(0) )
      .attr( "x2", (d,i)=>scX(i) ).attr( "y2", d=>scY(d) );

    svg.on( "click", function() {                              
      [ ds1, ds2 ] = [ ds2, ds1 ];                          
        
      svg.selectAll( "line" ).data( ds1 )                      
          .transition().duration( 500 ).delay( (d,i)=>200*i )  
          .attr( "y2", d=>scY(d) );                            
    });
}

onMounted(() => {
  makeStagger()
})
</script>



---

## Transition code


```js {*|2-5|7-9|11-14|16-17|19|20|21|*}

  function makeStagger() {
    let ds1 = [ 2, 1, 3, 5, 7, 8, 9, 9, 9, 8, 7, 5, 3, 1, 2 ];   
    let ds2 = [ 8, 9, 8, 7, 5, 3, 2, 1, 2, 3, 5, 7, 8, 9, 8 ];
    let n = ds1.length;
    let mx = d3.max( d3.merge( [ds1, ds2] ) ); 
    
    const svg = d3.select( "#stagger" );
    const scX = d3.scaleLinear().domain( [0,n] ).range( [50,540] );
    const scY = d3.scaleLinear().domain( [0,mx] ).range( [250,50] );

    svg.selectAll( "line" ).data( ds1 ).enter().append( "line" )  
      .attr( "stroke", "rgb(252, 28, 192)" ).attr( "stroke-width", 20 )
      .attr( "x1", (d,i) => scX(i) ).attr( "y1", scY(0) )
      .attr( "x2", (d,i) => scX(i) ).attr( "y2", d => scY(d) );

    svg.on( "click", function() {                              
      [ ds1, ds2 ] = [ ds2, ds1 ];                          
        
      svg.selectAll( "line" ).data( ds1 )                      
          .transition().duration( 1000 ).delay( (d,i) => 200 * i )  
          .attr( "y2", d=>scY(d) );                            
    });
  }

```

<!-- 
[click] 1. 先定義資料集，mx 是 ds1 和 ds2 中的最大值，用於設置 Y 軸比例尺的上限。使用了 d3.merge 將 ds1 和 ds2 合併成一個陣列，再找出最大值。

[click] 2. 創建 svg 與比例尺，scX 為水平比例尺，將數據索引（0 ~ n）映射到畫布的 X 軸範圍（50 ~ 540），計算每個 bar 的水平位置。

scY 為垂直比例尺，將數據值（0 ~ mx）映射到畫布的 Y 軸範圍（250 ~ 50），用於計算每個 bar 的高度。

[click] 3. 建立長條圖，y1 = Y 軸座標的起點、y2 = Y 軸座標的終點

[click] 4. 建立 click 事件，點擊後交換 ds1 和 ds2 的 data

[click] 5. 更新 bar 綁定新的數據。

[click] 6. 建立 transition 實例，duration 為每個 bar 的動畫持續時間為 500 毫秒，delay 為每個 bar 的動畫開始時間一次延遲 200 毫秒。

[click] 7. 更新 bar 的終點高度 = y2。 

-->


---
layout: fact
mdc: true
---

<div style="font-weight: bold">Time to eat !</div>