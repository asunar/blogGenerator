---
title: Perils of IsNull Function
date: 2018-12-10 08:13:48
tags:
---

Did you know that SQL Server’s IsNull() function does an implicit conversion?

<figure class="highlight sql">

<table>

<tbody>

<tr>

<td class="gutter">

<pre><span class="line">1</span>  
</pre>

</td>

<td class="code">

<pre><span class="line">ISNULL ( check_expression , replacement_value )</span>  
</pre>

</td>

</tr>

</tbody>

</table>

</figure>

The documentation explicitly states that

The value of check_expression is returned if it is not NULL; otherwise, replacement_value is returned after **it is implicitly converted to the type of check_expression, if the types are different. replacement_value can be truncated if replacement_value is longer than check_expression.** (emphasis mine)

<figure class="highlight sql">

<table>

<tbody>

<tr>

<td class="gutter">

<pre><span class="line">1</span>  
<span class="line">2</span>  
<span class="line">3</span>  
<span class="line">4</span>  
<span class="line">5</span>  
<span class="line">6</span>  
<span class="line">7</span>  
<span class="line">8</span>  
<span class="line">9</span>  
<span class="line">10</span>  
<span class="line">11</span>  
<span class="line">12</span>  
<span class="line">13</span>  
<span class="line">14</span>  
<span class="line">15</span>  
<span class="line">16</span>  
<span class="line">17</span>  
<span class="line">18</span>  
<span class="line">19</span>  
</pre>

</td>

<td class="code">

<pre><span class="line"><span class="keyword">DECLARE</span> @TestIsNull <span class="keyword">table</span> (</span>  
 <span class="line">EmpID <span class="built_in">varchar</span>(<span class="number">10</span>)</span>  
<span class="line">)</span>  
<span class="line"></span>  
<span class="line"><span class="keyword">insert</span> <span class="keyword">into</span> @TestIsNull</span>  
<span class="line"><span class="keyword">select</span> <span class="string">'1234567890'</span></span>  
<span class="line"></span>  
<span class="line"><span class="keyword">DECLARE</span> @empIdFilter <span class="built_in">varchar</span>(<span class="number">9</span>) = <span class="literal">null</span></span>  
<span class="line"></span>  
<span class="line"><span class="keyword">select</span> *</span>  
<span class="line"><span class="keyword">from</span></span>   
 <span class="line">@TestIsNull</span>   
<span class="line"><span class="keyword">where</span></span>  
 <span class="line">empId = <span class="keyword">ISNULL</span>(@empIdFilter,empid)</span>  
<span class="line"></span>  
<span class="line">EmpID</span>  
<span class="line"><span class="comment">----------</span></span>  
<span class="line"></span>  
<span class="line">(<span class="number">0</span> <span class="keyword">rows</span> affected)</span>  
</pre>

</td>

</tr>

</tbody>

</table>

</figure>

See the problem? Data type of EmpId column on line 2 is varchar(10) and the data type of empIdFilter is varchar(9).

Therefore, ISNULL will convert varchar(10) to varchar(9) resulting in truncation. Now imagine this in a stored procedure where the EmpId column datatype is not obvious and filter variable is declared somewhere far away from the SQL statement. It becomes a pretty insidious bug.

In order to avoid this, you can write your SQL statement like this.

<figure class="highlight sql">

<table>

<tbody>

<tr>

<td class="gutter">

<pre><span class="line">1</span>  
<span class="line">2</span>  
<span class="line">3</span>  
<span class="line">4</span>  
<span class="line">5</span>  
<span class="line">6</span>  
<span class="line">7</span>  
<span class="line">8</span>  
<span class="line">9</span>  
<span class="line">10</span>  
<span class="line">11</span>  
<span class="line">12</span>  
<span class="line">13</span>  
<span class="line">14</span>  
<span class="line">15</span>  
<span class="line">16</span>  
<span class="line">17</span>  
<span class="line">18</span>  
<span class="line">19</span>  
<span class="line">20</span>  
</pre>

</td>

<td class="code">

<pre><span class="line"><span class="keyword">DECLARE</span> @TestIsNull <span class="keyword">table</span> (</span>  
 <span class="line">EmpID <span class="built_in">varchar</span>(<span class="number">10</span>)</span>  
<span class="line">)</span>  
<span class="line"></span>  
<span class="line"><span class="keyword">insert</span> <span class="keyword">into</span> @TestIsNull</span>  
<span class="line"><span class="keyword">select</span> <span class="string">'1234567890'</span></span>  
<span class="line"></span>  
<span class="line"><span class="keyword">DECLARE</span> @empIdFilter <span class="built_in">varchar</span>(<span class="number">9</span>) = <span class="literal">null</span></span>  
<span class="line"></span>  
<span class="line"><span class="keyword">select</span> *</span>  
<span class="line"><span class="keyword">from</span></span>   
 <span class="line">@TestIsNull</span>   
<span class="line"><span class="keyword">where</span></span>   
 <span class="line">@empIdFilter <span class="keyword">is</span> <span class="literal">null</span> <span class="keyword">or</span> empid = @empIdFilter</span>  
<span class="line"></span>  
<span class="line">EmpID</span>  
<span class="line"><span class="comment">----------</span></span>  
<span class="line"><span class="number">1234567890</span></span>  
<span class="line"></span>  
<span class="line">(<span class="number">1</span> <span class="keyword">row</span> affected)</span>  
</pre>

</td>

</tr>

</tbody>

</table>

</figure>
