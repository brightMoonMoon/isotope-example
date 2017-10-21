# isotope的基本用法
### Filter
#### selectors
>过滤项目的最简单方法是使用选择器
```html
   <div class="grid">
    <div class="item-one"></div>
    <div class="item-two"></div>
    <div class="item-three"></div>
   </div>

  <script>
    var $grid = $('.grid');
    $gri.isotope(filter:'.item-one')  /*选中类为item-one的元素，其余的都隐藏*/
    $grid.isotope(filter:'.item-one,.item-two')/*两项都选中，都显示，其余的隐藏*/
    $grid.isotope(filter:'*')/*选中类grid下的全部元素，并设为显示*/
    </script>
```

#### Functions
>您可以使用功能进行过滤
```
    <div class="grid">
    <div class="item-one number">88</div>
    <div class="item-two number">23</div>
    <div class="item-three number">123</div>
   </div>
   <script>
    var $grid = $('.grid');
    $grid.isotope({
        filter:function(){
            var number = $(this).filter('.number').text();
            /*这个我想着就是上说的find()方法，这里得用filter()求吊大的指点一下*/[官方文档](https://isotope.metafizzy.co/filtering.html)
            return parseInt(number,10)>50;
            /*过滤出数字大于50的元素项进行显示*/
        }
    })
    </script>
    <!-- no use jquery  -->
    <script>
        iso.arrange({
            filter:function(itemElem){
                var number = itemElem.querySelector(.number').innerText;
                return parseInt(number,10)>50
            }
        })
    </script>
``
```

#### UI
>UI是我们经常使用的,所以又想用这个插件的同学请多看看这一部分哦。
```HTML
   <div class="button-group">
       <button data-filter="*">show all</button>
       <button data-filter=".item-one">show one</button>
       <button data-filter=".item-one .item-two">show two</button>
   </div>
   <div>
       <div class="item-one">one</div>
       <div class="item-two">two</div>
       <div class="item-three">three</div>
    </div>
    <script>
        var $grid = $('.grid').isotope({
             // options  进行初始化
        });
        $('.button-group').on(''click','button',function{
             var filterValue = $(this).attr('data-filter');
              $grid.isotope({ filter: filterValue });
        })
    </script>
```
#### Filter Functions 
>与上一个functions 不同的是,这个是对数据处理过后，再用filter过滤
```html
    <div class="button-group">
        <button data-filter="numberGreaterThan50">number > 50</button>
        <button data-filter="ium">name ends with -ium</button>
    </div>
    <div class="grid">
        <div class="number">99</div>
        <div class="name">22222ium</div>
    </div>
<script>
    var $grid = $('.grid').isotope({});
    var filterFns = {
        // show if number is greater than 50
        numberGreaterThan50: function () {
            var number = $(this).filter('.number').html();
            console.log(this)
            return parseInt(number, 10) > 50;
        },
        // show if name ends with -ium
        ium: function () {
            var name = $(this).filter('.name').text();
            return name.match(/ium$/);
        }
    };

    // filter items on button click
    $('.button-group').on('click', 'button', function () {
        var filterValue = $(this).attr('data-filter');
        // use filter function if value matches
        filterValue = filterFns[filterValue] || filterValue;
        $grid.isotope({ filter: filterValue });
    });
</script>
```
### Sorting
>用于对元素更详尽的过滤操作
``` html
<div class="grid">
  <div class="element-item transition metal" data-category="transition">
    <p class="number">79</p>
    <h3 class="symbol">Au</h3>
    <h2 class="name">Gold</h2>
    <p class="weight">196.966569</p>
  </div>
  <div class="element-item metalloid" data-category="metalloid">
    <p class="number">51</p>
    <h3 class="symbol">Sb</h3>
    <h2 class="name">Antimony</h2>
    <p class="weight">121.76</p>
  </div>
</div>
<!-- 以上每个div项目中都有几个可排序的数据点-->
<script>
   var $grid = $('.grid').isotope({
    getSortData: {
    name: '.name',
    symbol: '.symbol',
    number: '.number parseInt',
    category: '[data-category]',
    weight: function( itemElem ) {
      var weight = $( itemElem ).find('.weight').text();
      return parseFloat( weight.replace( /[\(\)]/g, '') );
    }
  } 
  /*对象的键使用用于排序的关键字。对象值是用于检索数据的快捷方式字符串或函数
  一：name:'.name',该字符串将被用作查询选择器
  二：category:'[data-category]',用来获取属性的值
  三：number:'.number parseInt'通过向快捷方式字符串中添加解析器关键字，数据值可以解析成数字
  四：getSortData函数,如上面的weight函数此功能用于每个项目元素以获取数据。该函数提供了一个参数itemElem，它是item元素。该函数需要返回数据点。
    */
});

// sort items on button click
$('.sort-by-button-group').on( 'click', 'button', function() {
  var sortByValue = $(this).attr('data-sort-by');
  $grid.isotope({ sortBy: sortByValue });
});
/*排序方式，对于每个设置的属性getSortData,isotope都可以使用该sortBy选项进行排序,sortBy可以设置为以上'name','symbol'
ps：如果想进行多重分类，就将sortBy()设置为数据，这样就可以进行多重分类*/
</script>
```
#### sortAscending
>这个选项用来设置升序还是排序

默认情况下,是升序的，若是想降序，需要将sortAscending设置为**sortAscending:false**也可以设置每个sortBy值设置升序
```JQ
$grid.isotope({
  sortAscending: {
    name: true,
    weight: false,
    category: true,
    number: false
  }
});
```
#### updateSortData
>要在对项目元素进行更改后更新排序数据，请使用该updateSortData方法。
```jq
   $grid.isotope('updateSortData').isotope();
```
以下有两个例子，分别是filer和sorting，我把这个例子放到git上，有意者可以上git上下载查看学习。
[传送门，如果对你有帮助，别忘记点赞哦]()
 ---

 其下的就是利用isotope进行layout方面的操作，等过两天有空再把isotope使用姿势（2）更新出来吧。  
	 　　谢谢大家观看，如有bug，请指正出来，共同进步。
