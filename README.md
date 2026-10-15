# .github.io
城市天气问候小工具
[index.html](https://github.com/user-attachments/files/33473429/index.html)
<!doctype html>
<html lang="zh-CN">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>城市信息 · 天气 · 早安问候助手</title>
<style>
*{box-sizing:border-box}
body{
    margin:0;
    background:#f2f6fb;
    color:#1f2937;
    font-family:system-ui,"Microsoft YaHei",PingFang SC,sans-serif
}
.wrap{max-width:900px;margin:auto;padding:14px}
.hero{
    padding:24px;
    border-radius:18px;
    color:white;
    background:linear-gradient(130deg,#166534,#0f766e,#2563eb)
}
h1{margin:0 0 8px;font-size:28px}
.hero p,.small{line-height:1.65}
.card{
    background:#fff;
    border-radius:15px;
    padding:18px;
    margin:14px 0;
    box-shadow:0 5px 20px #0000000a
}
h2{font-size:20px;margin:0 0 13px}
.search{display:flex;gap:8px}
input{
    flex:1;
    min-width:0;
    border:1px solid #cbd5e1;
    border-radius:10px;
    padding:13px;
    font-size:16px
}
button{
    border:0;
    border-radius:10px;
    background:#2563eb;
    color:white;
    padding:11px 15px;
    font-size:14px;
    cursor:pointer
}
button:disabled{opacity:0.55;cursor:not-allowed;}
.secondary{background:#eef2f7;color:#334155}
.grid{display:grid;grid-template-columns:repeat(3,1fr);gap:9px}
.tile{
    padding:12px;
    border:1px solid #e2e8f0;
    background:#f8fafc;
    border-radius:11px;
    min-height:76px
}
.k{font-size:12px;color:#64748b;margin-bottom:6px}
.v{font-weight:700;line-height:1.5}
.cols{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-top:10px}
.small{font-size:13px;color:#64748b}
.status{font-size:13px;color:#64748b;margin-top:9px}
.toolbar{display:flex;gap:8px;flex-wrap:wrap;margin:10px 0}
.tag{
    display:inline-block;
    background:#ecfdf5;
    color:#047857;
    border-radius:20px;
    padding:5px 10px;
    margin:3px;
    cursor:pointer;
}
.greeting{
    display:flex;
    gap:10px;
    border:1px solid #e2e8f0;
    border-radius:12px;
    padding:12px;
    margin:9px 0
}
.num{
    background:#dbeafe;
    color:#1d4ed8;
    border-radius:8px;
    min-width:28px;
    height:28px;
    display:grid;
    place-items:center;
    font-weight:bold
}
.gtext{white-space:pre-wrap;line-height:1.75}
.actions{margin-top:7px}
.chip-list{display:flex;flex-wrap:wrap;gap:6px;margin:8px 0;}
.chip{
    border:1px solid #cbd5e1;
    background:#f8fafc;
    border-radius:99px;
    font-size:12px;
    padding:4px 9px;
}
.place-tag{
    background:#edf2fb;
    color:#1d4ed8;
    padding:5px 8px;
    border-radius:8px;
    font-size:12px;
    margin:3px;
    display:inline-block;
}
.error-text{color:#b91c1c;font-size:13px;margin-top:8px;display:none;}
.warn-notice{background:#fffbeb;border:1px solid #fcd34d;border-radius:10px;padding:10px;font-size:13px;color:#92400e;margin-bottom:12px;}
.toast{
    position:fixed;
    bottom:24px;
    left:50%;
    transform:translateX(-50%);
    background:#1f2937;
    color:#fff;
    padding:10px 18px;
    border-radius:10px;
    font-size:13px;
    opacity:0;
    pointer-events:none;
    transition:opacity 0.22s;
}
.toast.show{opacity:1;}
@media(max-width:600px){
    .grid{grid-template-columns:repeat(2,1fr)}
    .cols{grid-template-columns:1fr}
    .search{flex-direction:column}
    .search button{width:100%}
    h1{font-size:24px}
    .card{padding:14px}
}
</style>
</head>
<body>
<div class="toast" id="toast">已复制</div>
<main class="wrap">
<div class="warn-notice">💡提示：本页面必须部署在HTTPS网页服务（Github‑Pages/Vercel/Netlify）下使用；直接双击本地html打开会被浏览器阻止全部网络请求。</div>
<header class="hero">
    <h1>城市信息 · 天气 · 早安问候助手</h1>
    <p>填写地区名称，查询当日天气、地方文化与旅游线索，并生成 10 条简洁、平实、有温度的上午问候。海外网络可正常访问。</p>
</header>

<section class="card">
    <h2>① 输入地区</h2>
    <div class="search">
        <input id="place" value="马六甲" placeholder="例如：杭州、成都、马来西亚马六甲">
        <button id="go">查询并生成</button>
    </div>
    <div class="small">建议填写“城市 + 省份/国家”，提高地区识别准确度。</div>
    <div id="status" class="status">等待查询。天气与维基百科资料需要联网。</div>
    <div id="errorText" class="error-text"></div>
    <div class="small" style="margin-top:8px">热门：
        <span class="tag" data-city="北京">北京</span>
        <span class="tag" data-city="上海">上海</span>
        <span class="tag" data-city="成都">成都</span>
        <span class="tag" data-city="杭州">杭州</span>
        <span class="tag" data-city="马六甲">马六甲</span>
    </div>
</section>

<section class="card">
    <h2>② 当日天气</h2>
    <div class="grid">
        <div class="tile"><div class="k">当前气温</div><div id="temp" class="v">—</div></div>
        <div class="tile"><div class="k">天气状况</div><div id="cond" class="v">—</div></div>
        <div class="tile"><div class="k">体感温度</div><div id="feel" class="v">—</div></div>
        <div class="tile"><div class="k">今日最高 / 最低</div><div id="range" class="v">—</div></div>
        <div class="tile"><div class="k">降雨概率</div><div id="rain" class="v">—</div></div>
        <div class="tile"><div class="k">风速</div><div id="wind" class="v">—</div></div>
    </div>
    <p id="weatherNote" class="small">天气提示会在查询后显示。</p>
</section>

<section class="card">
    <h2>③ 地方特色与旅游参考</h2>
    <div class="grid">
        <div class="tile"><div class="k">地区 / 国家</div><div id="region" class="v">—</div></div>
        <div class="tile"><div class="k">人口 / 城市规模</div><div id="population" class="v">暂无数据</div></div>
        <div class="tile"><div class="k">海拔</div><div id="elev" class="v">—</div></div>
    </div>
    <div class="cols">
        <div class="tile">
            <b>城市标签</b>
            <div id="cityTags" class="chip-list">
                <span class="small">查询城市后展示标签</span>
            </div>
        </div>
        <div class="tile">
            <b>推荐走访地点</b>
            <div id="attractionsBox">
                <span class="small">查询后展示景点</span>
            </div>
        </div>
    </div>
    <div style="margin-top:10px;">
        <b>城市简介（维基百科）</b>
        <p id="cityIntro" class="small">城市人文介绍将显示在这里。</p>
    </div>
    <div class="toolbar" id="searchLinks">
        <!-- 动态生成外部搜索链接：百度中文搜索 / DuckDuckGo国际搜索，兼顾国内海外网络查阅 -->
    </div>
</section>

<section class="card">
    <h2>④ 上午问候语（10 条不同方向）</h2>
    <p id="intro" class="small">问候语结合天气、气温与地区生成。</p>
    <div class="toolbar">
        <button id="regenerateBtn">重新生成 10 条</button>
        <button class="secondary" id="copyAllBtn">复制全部问候</button>
    </div>
    <div id="greetings">
        <div class="small" style="padding:12px;text-align:center;">选择地区后生成早安问候</div>
    </div>
</section>

<footer class="small">
天气接口：Open‑Meteo（全球CORS支持，海外可访问）；百科资料：Wikimedia MediaWiki API；内置国内城市资料库。信息仅供参考，不替代官方天气预警。
</footer>
</main>

<script>
const $=s=>document.querySelector(s);
let abortController=null;
let currentCity=null;
let weatherResult=null;
let offset=0;

// 内置国内城市资料库
const cityData={
'北京':{tags:['京味儿','胡同','皇家园林'],intro:'古都的厚重与当代的节奏并行。胡同烟火、城墙遗迹与开阔的城市中轴，共同写出北京的日常。',places:['故宫博物院','天坛','颐和园','八达岭长城'],scale:'约2184万',scaleLabel:'常住人口（2023）',size:'超大城市'},
'上海':{tags:['海派文化','江南','滨江'],intro:'江南水乡底色与国际都市气质交织。外滩、弄堂和梧桐街区，映照出开放而细腻的城市生活。',places:['外滩','豫园','上海博物馆','朱家角古镇'],scale:'约2487万',scaleLabel:'常住人口（2023）',size:'超大城市'},
'广州':{tags:['广府文化','早茶','花城'],intro:'岭南文化与商贸活力汇聚于此。早茶、骑楼与珠江夜色，构成广州鲜活而包容的生活气息。',places:['广州塔','陈家祠','白云山','沙面'],scale:'约1883万',scaleLabel:'常住人口（2023）',size:'超大城市'},
'深圳':{tags:['创新之城','滨海','年轻活力'],intro:'从海湾渔村发展为创新之城，深圳以开放、务实和多元的城市文化闻名。',places:['世界之窗','深圳湾公园','大鹏所城','莲花山公园'],scale:'约1779万',scaleLabel:'常住人口（2023）',size:'超大城市'},
'成都':{tags:['巴蜀文化','茶馆','熊猫'],intro:'安逸与创造力并存的天府之城。茶馆闲谈、川菜香气和蜀地历史，塑造了成都松弛而有劲的性格。',places:['武侯祠','杜甫草堂','青城山','大熊猫繁育研究基地'],scale:'约2140万',scaleLabel:'常住人口（2023）',size:'超大城市'},
'杭州':{tags:['江南水乡','西湖','数字经济'],intro:'西湖山水与江南文脉相映，古典诗意和数字产业在这里并肩生长。',places:['西湖','灵隐寺','良渚古城遗址','西溪国家湿地公园'],scale:'约1252万',scaleLabel:'常住人口（2023）',size:'特大城市'},
'南京':{tags:['六朝古都','金陵','梧桐'],intro:'六朝文脉与长江气象交汇，城墙、梧桐和书卷气，留下南京沉静而坚韧的城市印象。',places:['中山陵','明孝陵','夫子庙秦淮河','南京城墙'],scale:'约954万',scaleLabel:'常住人口（2023）',size:'特大城市'},
'西安':{tags:['十三朝古都','秦腔','面食'],intro:'周秦汉唐的历史层叠于城墙内外，市井烟火与古都遗迹共同构成西安的厚重底色。',places:['秦始皇帝陵博物院','大雁塔','西安城墙','陕西历史博物馆'],scale:'约1307万',scaleLabel:'常住人口（2023）',size:'特大城市'},
'武汉':{tags:['江湖气','码头文化','热干面'],intro:'江汉交汇、湖泊环绕，码头文化带来直爽豪迈的城市气质，书香与烟火都很浓。',places:['黄鹤楼','东湖','湖北省博物馆','昙华林'],scale:'约1377万',scaleLabel:'常住人口（2023）',size:'超大城市'},
'重庆':{tags:['山城','火锅','两江交汇'],intro:'依山而建、临江而兴，立体街巷与码头文化塑造出重庆热烈、直爽的生活气息。',places:['洪崖洞','大足石刻','长江索道','武隆喀斯特'],scale:'约3191万',scaleLabel:'常住人口（2023，全市）',size:'超大城市'},
'苏州':{tags:['园林之城','吴文化','水巷'],intro:'古典园林、运河水巷与精巧手艺，让苏州在温婉之外也有着鲜明的创造力。',places:['拙政园','留园','周庄古镇','苏州博物馆'],scale:'约1291万',scaleLabel:'常住人口（2023）',size:'特大城市'},
'景德镇':{tags:['千年瓷都','陶瓷','匠心'],intro:'瓷器制造与手工艺传统延续千年，窑火、泥土和匠人故事构成这座城市的独特气质。',places:['景德镇中国陶瓷博物馆','古窑民俗博览区','瑶里古镇','陶溪川'],scale:'约162万',scaleLabel:'常住人口（2023）',size:'中等城市'},
'厦门':{tags:['闽南文化','海岛','骑楼'],intro:'海风、骑楼与闽南文化相伴，城市节奏舒展，也保留着鲜明的港口记忆。',places:['鼓浪屿','南普陀寺','环岛路','厦门大学'],scale:'约532万',scaleLabel:'常住人口（2023）',size:'Ⅱ型大城市'},
'青岛':{tags:['海滨城市','啤酒文化','红瓦绿树'],intro:'山海相拥的港口城市，海滨风光、近代建筑和鲜明的海洋文化彼此映衬。',places:['崂山','八大关','栈桥','青岛啤酒博物馆'],scale:'约1037万',scaleLabel:'常住人口（2023）',size:'特大城市'},
'长沙':{tags:['湖湘文化','湘菜','媒体艺术'],intro:'湖湘文脉、湘江风光与热辣市井交织，长沙的热情既在街巷，也在山水之间。',places:['岳麓山','橘子洲','湖南省博物馆','天心阁'],scale:'约1051万',scaleLabel:'常住人口（2023）',size:'特大城市'}
};

// 真实emoji
const emojis=['😊','💪','👍','🌿','😄','😃','👏','😉','🎵','⭐','🎉','😆','❤️','🤔','🙂','🥳','😎','🙋','🏠','🎁','✨','💖','🔥','🤸','📢','👀','🎶','💛','😂','🎊','🌟','🤤','🚀','🤧'];

// 天气文字映射 Open‑Meteo code
function weatherText(code){
    const map={
        0:"晴朗",1:"大致晴朗",2:"局部多云",3:"阴天",
        45:"有雾",48:"雾凇",51:"毛毛雨",53:"小雨",55:"较强毛毛雨",
        61:"小雨",63:"中雨",65:"大雨",71:"小雪",73:"中雪",75:"大雪",
        80:"阵雨",81:"较强阵雨",82:"强阵雨",95:"雷雨",96:"雷雨伴冰雹",99:"强雷雨伴冰雹"
    };
    return map[code]||"天气情况待确认";
}

function fmtTemp(val){
    if(val===null||val===undefined) return "—";
    return Math.round(val)+"℃";
}

function toast(msg){
    const t=$("#toast");
    t.textContent=msg;
    t.classList.add("show");
    setTimeout(()=>t.classList.remove("show"),1600);
}

// 维基百科中文API，海外网络支持CORS origin=*
async function fetchWikiSummary(cityName){
    try{
        const url=`https://zh.wikipedia.org/w/api.php?action=query&generator=search&gsrsearch=${encodeURIComponent(cityName)}&gsrnamespace=0&gsrlimit=3&prop=extracts&exintro=1&explaintext=1&format=json&origin=*`;
        const resp=await fetch(url);
        if(!resp.ok) return null;
        const json=await resp.json();
        const pages=Object.values(json.query?.pages||{});
        if(!pages.length) return null;
        const best=pages.find(p=>p.title.includes(cityName))||pages[0];
        let text=(best.extract||"").replace(/\s+/g," ").trim();
        if(text.length>650) text=text.slice(0,650)+"……";
        return text||null;
    }catch(err){
        return null;
    }
}

// 查询地区+天气
async function loadPlace(){
    const name=$("#place").value.trim();
    if(!name){alert("请填写地区名称");return;}
    const goBtn=$("#go");
    $("#errorText").style.display="none";
    goBtn.disabled=true;
    $("#status").textContent="正在查询地区、天气、百科资料……";

    if(abortController) abortController.abort();
    abortController=new AbortController();
    const sig=abortController.signal;

    // 清空UI
    $("#searchLinks").innerHTML="";
    $("#cityIntro").textContent="正在请求维基百科简介……";

    try{
        // 1 地理编码 Open‑Meteo
        const geoRes=await fetch(`https://geocoding-api.open-meteo.com/v1/search?name=${encodeURIComponent(name)}&count=8&language=zh&format=json`,{signal});
        if(!geoRes.ok) throw new Error("地区查询服务不可用");
        const geoJson=await geoRes.json();
        if(!geoJson.results||!geoJson.results.length) throw new Error("未找到地区，请补充省份或国家名称");

        const loc=geoJson.results.find(x=>x.name.toLowerCase()===name.toLowerCase())||geoJson.results[0];

        // 2 天气
        const params=new URLSearchParams({
            latitude:loc.latitude,
            longitude:loc.longitude,
            current:"temperature_2m,apparent_temperature,weather_code,wind_speed_10m",
            daily:"temperature_2m_max,temperature_2m_min,precipitation_probability_max",
            timezone:"auto",
            forecast_days:"1"
        });
        const weatherRes=await fetch(`https://api.open-meteo.com/v1/forecast?${params.toString()}`,{signal});
        if(!weatherRes.ok) throw new Error("天气查询失败");
        const wJson=await weatherRes.json();

        weatherResult=wJson;
        currentCity={
            name:loc.name,
            admin1:loc.admin1||"",
            country:loc.country||"",
            lat:loc.latitude,
            lon:loc.longitude,
            elevation:loc.elevation
        };

        // 并行拉取维基百科简介
        const wikiText=await fetchWikiSummary(loc.name);

        const curr=wJson.current;
        const daily=wJson.daily;

        // 填充天气面板
        $("#temp").textContent=fmtTemp(curr.temperature_2m);
        $("#cond").textContent=weatherText(curr.weather_code);
        $("#feel").textContent=fmtTemp(curr.apparent_temperature);
        $("#range").textContent=`${fmtTemp(daily.temperature_2m_max[0])} / ${fmtTemp(daily.temperature_2m_min[0])}`;
        $("#rain").textContent=daily.precipitation_probability_max?.[0]!=null ? daily.precipitation_probability_max[0]+"%" : "—";
        $("#wind").textContent=curr.wind_speed_10m!=null ? Math.round(curr.wind_speed_10m)+" km/h" : "—";
        $("#region").textContent=[loc.name,loc.admin1,loc.country].filter(Boolean).join(" · ");
        $("#elev").textContent=loc.elevation!=null ? Math.round(loc.elevation)+" 米":"—";

        // 城市资料库渲染
        const cityInfo=cityData[loc.name];
        if(cityInfo){
            $("#population").textContent=`${cityInfo.scale}｜${cityInfo.size}`;
            $("#cityTags").innerHTML=cityInfo.tags.map(t=>`<span class="chip">${escapeHtml(t)}</span>`).join("");
            $("#attractionsBox").innerHTML=cityInfo.places.map(p=>`<span class="place-tag">${escapeHtml(p)}</span>`).join("");
        }else{
            $("#population").textContent="该地区不在本地资料库";
            $("#cityTags").innerHTML=`<span class="chip">${escapeHtml(loc.country||"境外地区")}</span>`;
            $("#attractionsBox").innerHTML=`<span class="small">暂无内置景点数据</span>`;
        }

        // 维基百科简介结果
        if(wikiText){
            $("#cityIntro").textContent=wikiText;
        }else{
            $("#cityIntro").textContent="维基百科未检索到该城市简介，请使用下方外部搜索链接查阅更多资料。";
        }

        // 生成外部搜索链接：兼顾国内（百度）、海外网络（DuckDuckGo）
        const term=encodeURIComponent(loc.name+" 城市 历史 景点");
        const linkWrap=$("#searchLinks");
        function addLink(text,url){
            const a=document.createElement("a");
            a.className="tag";
            a.target="_blank";
            a.rel="noopener noreferrer";
            a.href=url;
            a.textContent=text;
            linkWrap.appendChild(a);
        }
        addLink("百度中文搜索","https://www.baidu.com/s?wd="+term);
        addLink("DuckDuckGo国际搜索（海外优先）","https://duckduckgo.com/?q="+term);

        // 天气提示文案
        let noteText="出门前可结合实时天气安排活动。";
        const tempNow=curr.temperature_2m;
        if(tempNow>=32) noteText="天气较热，注意补水防晒。";
        else if(tempNow<=12) noteText="早晚偏凉，注意添衣。";
        $("#weatherNote").textContent=`当地时区：${wJson.timezone||"未知"}。${noteText}`;
        $("#intro").textContent=`已结合 ${loc.name} 当日天气（${weatherText(curr.weather_code)}，${fmtTemp(curr.temperature_2m)}）生成问候。`;

        offset=0;
        renderGreetings();
        $("#status").textContent=`查询完成，识别地区：${loc.name}（${loc.country||"无国家标注"}）`;
    }catch(err){
        if(err.name!=="AbortError"){
            $("#errorText").textContent="查询失败："+err.message;
            $("#errorText").style.display="block";
            $("#status").textContent="查询未完成";
        }
    }finally{
        goBtn.disabled=false;
    }
}

// 获取温度体感描述
function getTone(t){
    if(t<=5) return "寒意阵阵";
    if(t<=14) return "早上清凉";
    if(t<=23) return "体感舒适";
    if(t<=30) return "暖意融融";
    return "天气暑热";
}

// HTML转义防XSS
function escapeHtml(str){
    return String(str??"")
        .replaceAll("&","&amp;")
        .replaceAll("<","&lt;")
        .replaceAll(">","&gt;")
        .replaceAll('"',"&quot;")
        .replaceAll("'","&#39;");
}

// 渲染10条问候
function renderGreetings(){
    offset=offset%2000;
    if(!currentCity||!weatherResult) return;
    const cityName=currentCity.name;
    const curr=weatherResult.current;
    const tempVal=Math.round(curr.temperature_2m);
    const tone=getTone(tempVal);
    const weatherStr=weatherText(curr.weather_code);

    const templates=[
        `早上好，${cityName}。处在${tone}的清晨，愿你脚步踏实，稳步开启崭新一天。`,
        `早安！${cityName}今天约${tempVal}℃，认真对待平凡日常，愿手头顺利，心中有光。`,
        `${cityName}的清晨向你问好：${tone}，可以适当放慢节奏，但不要迷失心中方向，期待今日有所收获。`,
        `早上好！${cityName}此刻${weatherStr}，愿你内心从容，好好照顾自己的身体。`,
        `${cityName}今日${tempVal}℃，珍惜清晨时光，记得好好吃一顿早饭，元气面对生活。`,
        `早安，${cityName}的朋友。无论忙碌清闲，做事讲求章法，待人保有温度，平凡日子也能收获风景。`,
        `晨光问候！${cityName}${tone}提醒我们量力而行、脚踏实地，希望今天的付出都能有所回响。`,
        `早上好！${cityName}天气${weatherStr}，心怀期盼，脚下有力量，从容过好这一天。`,
        `${cityName}清晨，致敬每一位认真生活的人。${tempVal>=28?'记得补水避晒。':tempVal<=8?'出门记得添件外衣。':'根据气温增减衣物。'}愿今日平安顺心。`,
        `早安！祝愿生活在${cityName}的每一个人，日子安稳，心情舒畅，万事顺意。`
    ];
    const themes=["踏实前行","平凡日常","心怀方向","从容自守","惜时生活","待人温厚","脚踏实地","心怀期盼","关怀提醒","美好祝愿"];

    const greetWrap=$("#greetings");
    greetWrap.innerHTML="";
    const shift=offset%10;
    for(let i=0;i<10;i++){
        const realIdx=(i+shift)%10;
        const emoji=emojis[(offset*7+i)%emojis.length];
        const textLine=templates[realIdx]+" "+emoji;

        const div=document.createElement("div");
        div.className="greeting";
        div.innerHTML=`
            <div class="num">${i+1}</div>
            <div style="flex:1">
                <div class="gtext">${escapeHtml(textLine)}</div>
                <div class="actions">
                    <button class="secondary" data-idx="${i}">复制这条</button>
                </div>
            </div>
        `;
        greetWrap.appendChild(div);
    }
    // 单条复制绑定
    document.querySelectorAll(".greeting button").forEach(btn=>{
        btn.onclick=()=>{
            const gtext=btn.closest(".greeting").querySelector(".gtext").textContent;
            copyClip(gtext);
        }
    })
}

// 复制剪贴板，降级兼容
async function copyClip(text){
    try{
        await navigator.clipboard.writeText(text);
        toast("已复制");
    }catch(e){
        const ta=document.createElement("textarea");
        ta.value=text;
        document.body.appendChild(ta);
        ta.select();
        document.execCommand("copy");
        ta.remove();
        toast("已复制");
    }
}

// 全部复制
function copyAllGreet(){
    const list=[...document.querySelectorAll(".gtext")].map((el,i)=>`${i+1}. ${el.textContent}`);
    copyClip(list.join("\n\n"));
}

// 事件绑定
$("#go").addEventListener("click",loadPlace);
$("#place").addEventListener("keydown",e=>{if(e.key==="Enter") loadPlace()});
$("#regenerateBtn").addEventListener("click",()=>{offset++;renderGreetings()});
$("#copyAllBtn").addEventListener("click",copyAllGreet);

// 热门标签点击
document.querySelectorAll(".tag[data-city]").forEach(t=>{
    t.addEventListener("click",()=>{
        $("#place").value=t.dataset.city;
        loadPlace();
    })
})
</script>
</body>
</html>
