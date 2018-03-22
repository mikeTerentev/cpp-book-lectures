<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>virtual</title>
  <link rel="stylesheet" href="https://stackedit.io/style.css" />
</head>

<body class="stackedit">
  <div class="stackedit__html"><h1 id="наследование">Наследование</h1>
<h2 id="виртуальные-функции">Виртуальные функции</h2>
<p>Пусть у нас есть два класса :</p>
<pre class=" language-c"><code class="prism ++ language-c"> <span class="token keyword">struct</span> base<span class="token punctuation">{</span>
     <span class="token keyword">int</span> a<span class="token punctuation">;</span>
     <span class="token keyword">int</span> b<span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
</code></pre>
<pre class=" language-c"><code class="prism ++ language-c">  <span class="token keyword">struct</span> derived <span class="token punctuation">:</span> base<span class="token punctuation">{</span>
      <span class="token keyword">int</span> c<span class="token punctuation">;</span>
      ind d<span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
</code></pre>
<p><strong>base</strong> - базовый класс.<br>
<strong>devived</strong> - производный<br>
Компилятор ищет переменную сначала в производном классе, потом в базовом.</p>
<pre class=" language-c"><code class="prism ++ language-c">  derived <span class="token operator">&amp;</span>x <span class="token operator">=</span><span class="token punctuation">(</span>derived<span class="token operator">&amp;</span><span class="token punctuation">)</span>y <span class="token operator">-</span> ошибка
</code></pre>
<p>От производного к базовую мы может спокойно кастить.<br>
Динамический тип – то на что мы ссылаемся<br>
Кастить можно, если то ,что мы кастим, является  базовым типом динамического типа.</p>

<table>
<thead>
<tr>
<th><strong>base</strong></th>
<th>a</th>
<th>b</th>
<th></th>
<th></th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>derived</strong></td>
<td>a</td>
<td>b(base)</td>
<td>b(derived)</td>
<td>c</td>
</tr>
</tbody>
</table><pre class=" language-c"><code class="prism ++ language-c"> <span class="token keyword">struct</span> base<span class="token punctuation">{</span>
     <span class="token keyword">void</span> <span class="token function">f</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
     <span class="token keyword">void</span> <span class="token function">g</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
  <span class="token punctuation">}</span>
</code></pre>
<pre class=" language-c"><code class="prism ++ language-c">  <span class="token keyword">struct</span> derived <span class="token punctuation">:</span> base<span class="token punctuation">{</span>
      <span class="token keyword">void</span>  <span class="token function">g</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
      <span class="token keyword">void</span> <span class="token function">f</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
</code></pre>
<p>Компилятор ищет функцию сначала в производном классе, потом в базовом.</p>
<pre class=" language-c"><code class="prism ++ language-c"><span class="token comment">// in derived</span>
<span class="token keyword">void</span> <span class="token function">g</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">{</span>
base<span class="token punctuation">:</span><span class="token function">g</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>

d<span class="token punctuation">.</span><span class="token function">f</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token comment">// base:f </span>
d<span class="token punctuation">.</span>base<span class="token punctuation">:</span><span class="token punctuation">:</span><span class="token function">g</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// derived::g;</span>
</code></pre>
<p>Это означает, что я в  хочу вызвать функцию из определенного класса</p>
<h2 id="механизм-виртуалньой-функции">Механизм виртуалньой функции</h2>
<p><strong>virtual</strong> функция, которая нашлась бы в динамическом типе.</p>
<pre class=" language-c"><code class="prism ++ language-c"><span class="token keyword">struct</span> derived <span class="token punctuation">:</span> base<span class="token punctuation">{</span>
   virtual  <span class="token keyword">void</span>  <span class="token function">g</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
     <span class="token keyword">void</span> <span class="token function">f</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
 <span class="token punctuation">}</span>
 derived d<span class="token punctuation">;</span>
 base<span class="token operator">&amp;</span> b2 <span class="token operator">=</span> d<span class="token punctuation">;</span>
 b2<span class="token punctuation">.</span><span class="token function">f</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
 b2<span class="token punctuation">.</span><span class="token function">g</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">//derived ::g;</span>
 b2<span class="token punctuation">.</span><span class="token function">h</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h2 id="пример">Пример</h2>
<pre class=" language-c"><code class="prism ++ language-c"><span class="token keyword">struct</span> data_source <span class="token punctuation">{</span>
   virtual <span class="token keyword">void</span> <span class="token function">read</span><span class="token punctuation">(</span>buffer <span class="token operator">&amp;</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
   <span class="token comment">// virtual void read(buffer &amp;) = 0;   чисто виртуальная функция абстрактная </span>
   virtual <span class="token operator">~</span><span class="token function">data_source</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> 
   <span class="token comment">// без virtual его код некорректный</span>
<span class="token punctuation">}</span>

<span class="token keyword">struct</span>  local_file <span class="token punctuation">:</span> data_source <span class="token punctuation">{</span>
    <span class="token keyword">void</span> <span class="token function">read</span><span class="token punctuation">(</span>buffer<span class="token operator">&amp;</span><span class="token punctuation">)</span>
<span class="token punctuation">}</span>

<span class="token keyword">struct</span>  http_connection <span class="token punctuation">:</span> data_source <span class="token punctuation">{</span>
    <span class="token keyword">void</span> <span class="token function">read</span><span class="token punctuation">(</span>buffer<span class="token operator">&amp;</span><span class="token punctuation">)</span>
<span class="token punctuation">}</span>

<span class="token keyword">struct</span> player<span class="token punctuation">{</span>
  <span class="token function">player</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">{</span>
   	current_source <span class="token operator">=</span> nullptr<span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
   
  <span class="token keyword">void</span> <span class="token function">play_file</span><span class="token punctuation">(</span>std<span class="token punctuation">:</span><span class="token punctuation">:</span> string <span class="token keyword">const</span><span class="token operator">&amp;</span> str<span class="token punctuation">)</span><span class="token punctuation">{</span>
   	<span class="token function">stop</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
   	delete current_source<span class="token punctuation">;</span>
   	current_source <span class="token operator">=</span> new <span class="token function">local_file</span><span class="token punctuation">(</span>str<span class="token punctuation">)</span><span class="token punctuation">;</span>
   <span class="token punctuation">}</span>
   
  data_source<span class="token operator">*</span> current_source<span class="token punctuation">;</span>
<span class="token punctuation">}</span>
<span class="token keyword">void</span> <span class="token function">play</span><span class="token punctuation">(</span>data_source<span class="token operator">&amp;</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">int</span> <span class="token function">main</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    local_file <span class="token function">lf</span><span class="token punctuation">(</span><span class="token string">"1.flac"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token function">play</span><span class="token punctuation">(</span><span class="token keyword">if</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    http_connection <span class="token function">hc</span><span class="token punctuation">(</span><span class="token string">"2.flac"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token function">play</span><span class="token punctuation">(</span>hc<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<h3 id="protected">Protected</h3>
<p>Модификатор доступа –  доступ имеют только производные класса</p>
<pre class=" language-c"><code class="prism ++ language-c"><span class="token keyword">struct</span> x_doer<span class="token punctuation">{</span>
   virtaul <span class="token keyword">void</span> <span class="token function">do_x</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token operator">=</span><span class="token number">0</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">;</span>

<span class="token keyword">struct</span> derived <span class="token punctuation">:</span> x_doer<span class="token punctuation">{</span>
    <span class="token keyword">void</span> <span class="token function">do_x</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">;</span>

<span class="token keyword">struct</span> derived2<span class="token punctuation">:</span> x_doer<span class="token punctuation">{</span>
    <span class="token keyword">void</span> <span class="token function">do_x</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">;</span>

<span class="token keyword">struct</span> base<span class="token punctuation">{</span>
    <span class="token function">base</span><span class="token punctuation">(</span>x_doer<span class="token operator">&amp;</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">;</span>

</code></pre>
</div>
</body>

</html>
