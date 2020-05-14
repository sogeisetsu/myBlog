<div class="sect4">
<h5 id="aop-pointcuts-examples" tabindex="-1"><a class="anchor" href="#aop-pointcuts-examples"></a>Examples</h5>
<div class="paragraph">
<p>Spring AOP users are likely to use the <code>execution</code> pointcut designator the most often.
The format of an execution expression follows:</p>
</div>
<div class="literalblock">
<div class="content">
<pre>    execution(modifiers-pattern? ret-type-pattern declaring-type-pattern?name-pattern(param-pattern)
                throws-pattern?)</pre>
</div>
</div>
<div class="paragraph">
<p>All parts except the returning type pattern (<code>ret-type-pattern</code> in the preceding snippet),
the name pattern, and the parameters pattern are optional. The returning type pattern determines
what the return type of the method must be in order for a join point to be matched.
<code>*</code> is most frequently used as the returning type pattern. It matches any return
type. A fully-qualified type name matches only when the method returns the given
type. The name pattern matches the method name. You can use the <code>*</code> wildcard as all or
part of a name pattern. If you specify a declaring type pattern,
include a trailing <code>.</code> to join it to the name pattern component.
The parameters pattern is slightly more complex: <code>()</code> matches a
method that takes no parameters, whereas <code>(..)</code> matches any number (zero or more) of parameters.
The <code>(*)</code> pattern matches a method that takes one parameter of any type.
<code>(*,String)</code> matches a method that takes two parameters. The first can be of any type, while the
second must be a <code>String</code>. Consult the
<a href="https://www.eclipse.org/aspectj/doc/released/progguide/semantics-pointcuts.html">Language
Semantics</a> section of the AspectJ Programming Guide for more information.</p>
</div>
<div class="paragraph">
<p>The following examples show some common pointcut expressions:</p>
</div>
<div class="ulist">
<ul>
<li>
<p>The execution of any public method:</p>
<div class="literalblock">
<div class="content">
<pre>    execution(public * *(..))</pre>
</div>
</div>
</li>
<li>
<p>The execution of any method with a name that begins with <code>set</code>:</p>
<div class="literalblock">
<div class="content">
<pre>    execution(* set*(..))</pre>
</div>
</div>
</li>
<li>
<p>The execution of any method defined by the <code>AccountService</code> interface:</p>
<div class="literalblock">
<div class="content">
<pre>    execution(* com.xyz.service.AccountService.*(..))</pre>
</div>
</div>
</li>
<li>
<p>The execution of any method defined in the <code>service</code> package:</p>
<div class="literalblock">
<div class="content">
<pre>    execution(* com.xyz.service.*.*(..))</pre>
</div>
</div>
</li>
<li>
<p>The execution of any method defined in the service package or one of its sub-packages:</p>
<div class="literalblock">
<div class="content">
<pre>    execution(* com.xyz.service..*.*(..))</pre>
</div>
</div>
</li>
</ul>
</div>
</div>