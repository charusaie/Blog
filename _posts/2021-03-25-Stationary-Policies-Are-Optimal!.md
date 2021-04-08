
<html xmlns="http://www.w3.org/1999/xhtml" lang="" xml:lang="">
<head>
  <meta charset="utf-8" />
  <meta name="generator" content="pandoc" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes" />
  <title>Blog post no. 2</title>
  <style type="text/css">
      code{white-space: pre-wrap;}
      span.smallcaps{font-variant: small-caps;}
      span.underline{text-decoration: underline;}
      div.column{display: inline-block; vertical-align: top; width: 50%;}
      div {
      text-align: justify;
      text-justify: inter-word;
      }
      p {
      font-size: 22px;
      }
  </style>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS_CHTML-full" type="text/javascript"></script>
  <!--[if lt IE 9]>
    <script src="//cdnjs.cloudflare.com/ajax/libs/html5shiv/3.7.3/html5shiv-printshiv.min.js"></script>
  <![endif]-->
</head>
<body>

<p>I was reading <a href="https://rtheory.github.io/lecture-notes/planning-in-mdps/lec2/">this</a> lecture note by <a href="https://sites.ualberta.ca/~szepesva/">Csaba</a>, where he mentioned that there is a stationary policy with the same value for every policy. However, he just briefly gave a hint about the form of such a stationary policy. In this blog, mostly inspired by (Syed et al. 2008), I am going to write the proof of this claim using the notion of Bellman Flow Constraints.</p>
<p>The problem of Markov Decision Processes is to find the optimal set of actions <span class="math inline">\(a_t\in {\cal A}_{t}\)</span> in each time <span class="math inline">\(t\)</span> where we have states <span class="math inline">\(s\in {\cal S}_t\)</span>, to maximize the return <span class="math inline">\(R = \sum_{t=0}^{\infty} \gamma^t r(a_t|s_t)\)</span>, where <span class="math inline">\(r(s_t, a_t)\)</span> is an immediate discounted reward in time <span class="math inline">\(t\)</span> for current-state/chosen-action pair <span class="math inline">\((s_t, a_t)\)</span>. Furthermore, the environment response to the action is encoded as <span class="math inline">\(p(s_{t+1}|s_t, a_t)\)</span>. Such behavior for the environment and rewards makes us name this process a ’Markov Decision Process’.</p>
<p>The policy is the set of functions in each time <span class="math inline">\(\pi=\{\pi_t\}_t\)</span> from the previous history of states <span class="math inline">\((s_0, \ldots, s_t)\in {\cal S}_0\times \ldots \times {\cal S}_t\)</span> to distribution on actions, which by abuse of notation we refer to as <span class="math inline">\(\pi_t(a_t|s_1, \ldots, s_t)\)</span>. A policy is Markov, if the functions are only functions of the last state <span class="math inline">\(s_t\)</span>. If a policy is memoryless and <span class="math inline">\(\pi_1=\pi_2=\ldots\)</span>, then the policy is <span><strong>stationary</strong></span>. Further, we set a distribution <span class="math inline">\(\mu\)</span> on the initial states of the process.</p>

<img src="/Blog/assets/img/0D2617B2-D50F-4528-A41F-E966BB57444B.png" alt="Optimal Memoryless MDP solver" >

<p>To simplify our calculations, we define <span><strong>occupancy measure</strong></span> for a policy as follows: <span class="math display">\[\begin{aligned}
        v_{\mu}^{\pi}(s, a)= \sum_{t=0}^{\infty}\gamma^t p_{\mu}^{\pi} (s_t=s, a_t=a),
     \end{aligned}\]</span> where <span class="math inline">\(p_{\mu}^{\pi}(s_t=s, a_t=a)\)</span> is the probability that with the initial distribution <span class="math inline">\(\mu\)</span> and policy <span class="math inline">\(\pi\)</span>, in time <span class="math inline">\(t\)</span>, we face state <span class="math inline">\(s\)</span> and we take action <span class="math inline">\(t\)</span>.</p>
<p>Then, we can rewrite the expected return, also referred as value function for the policy <span class="math inline">\(\pi\)</span> and the initial distribution <span class="math inline">\(\mu\)</span>, as</p>
<p><span class="math display">\[\begin{aligned}
        v^{\pi}(\mu) &amp;= \mathbb{E}_{\mu, \pi}\Big[\sum_{t=0}^{\infty} \gamma^t r( s_t, a_t)\Big] = \sum_{t=0}^{\infty} \gamma^t \sum_{s, a}r( s, a) p(s_t=s, a_t=a)\\
        &amp;=  \sum_{s, a} r(s, a)\sum_{t=0}^{\infty}\gamma^t p(s_t=s, a_t=a) = \sum_{s, a} r(s, a) v_{\mu}^{\pi}(s, a)\\
        &amp;:= \langle r, v_{\mu}^{\pi}\rangle.
     \end{aligned}\]</span></p>
<p>Such a mathematical argument concludes one only needs to have the occupancy measure in hand to calculate the value function. Moreover, it shows that to optimize the value function one needs to align the occupancy measure vector to the reward vector as much as possible.</p>
<p>Then we go to the main problem, which is, for every policy <span class="math inline">\(\pi\)</span>, there is a stationary policy <span class="math inline">\(\overline{\pi}\)</span> with the same value function. Using above discussion, to prove such a statement, we only need to prove that the occupancy measure of <span class="math inline">\(\pi\)</span> and <span class="math inline">\(\overline{\pi}\)</span> are equal.</p>
<p>To show the equality, first we define <span><strong><span class="math inline">\(\overline{\pi}\)</span>-specific Bellman’s flow constraints</strong></span> for a function (vector) <span class="math inline">\(x(s, a)\)</span>, stationary policy <span class="math inline">\(\overline{\pi}(a|s)\)</span>, environment dynamics <span class="math inline">\(p(s&#39;|s, a)\)</span>, and the initial distribution <span class="math inline">\(\mu(s)\)</span> as below <span class="math display">\[\begin{aligned}
        x(s, a)&amp;\geq 0\\
        x(s, a)&amp;= \overline{\pi}(a|s)\mu(s) +\gamma \overline{\pi}(a|s)\sum_{s&#39;, a&#39;} x(s&#39;, a&#39;)p(s|s&#39;, a&#39;).
     \end{aligned}\]</span> <a name="eqn: pispecific"></a>  Further, we say <span class="math inline">\(x(s, a)\)</span> satisfies general form of <span><strong>Bellman’s flow constraints</strong></span> if <span class="math display">\[\begin{aligned}
        x(s, a)&amp;\geq 0\\
        \sum_{a}x(s, a)&amp;= \mu(s) +\gamma \sum_{s&#39;, a&#39;} x(s&#39;, a&#39;)p(s|s&#39;, a&#39;).
     \end{aligned}\]</span></p>
<p>Note: Every function that satisfies <span class="math inline">\(\pi\)</span>-specific Bellman’s flow constraints, also satisfies the general form of Bellman’s flow constraints.</p>
<p>Next, we proceed to prove the equality in the following steps:</p>
<ol>
<li><p><span><strong>The occupancy measure of a stationary policy <span class="math inline">\(\overline{\pi}(a|s)\)</span> is the unique solution of <span class="math inline">\(\pi\)</span>-specific Bellman’s flow constraints.</strong></span></p></li>
<li><p><span><strong>Every stationary policy <span class="math inline">\(\overline{\pi}(a|s)\)</span> and its occupancy measure <span class="math inline">\(v_{\mu}^{\overline{\pi}}(s, a)\)</span> satisfy the equation <span class="math inline">\(\overline{\pi}(a|s) = \frac{v_{\mu}^{\overline{\pi}}(s, a)}{\sum_{a} v_{\mu}^{\overline{\pi}}(s, a)}\)</span>.</strong> </span></p></li>
<li><p><span><strong>Every occupancy measure satisfies Bellman’s flow constraints.</strong></span></p></li>
<li><p><span><strong>Every function <span class="math inline">\(x(s, a)\)</span> that satisfies Bellman’s flow constraint is an occupancy measure of a stationary policy <span class="math inline">\(\overline{\pi}(a|s)=\frac{x(s, a)}{\sum_{a}x(s, a)}\)</span>.</strong> </span></p></li>
<li><p><span><strong>For every policy <span class="math inline">\(\pi\)</span>, the occupancy measure <span class="math inline">\(v_{\mu}^{\pi}(s, a)\)</span> is also the occupancy measure of stationary policy <span class="math inline">\(\overline{\pi}(s|a)=\frac{v_{\mu}^{\pi}(s, a)}{\sum_{a}v_{\mu}^{\pi}(s, a)}\)</span>.</strong></span></p></li>
</ol>
<p>Let’s analyze each of teh steps:</p>
<ol>
<li><p>First we prove the occupancy measure <span class="math inline">\(v_{\mu}^{\overline{\pi}}(s, a)\)</span> is a solution of <span class="math inline">\(\overline{\pi}\)</span>-specific Bellman’s constraints. We already know that the occupancy measure is positive. Hence, it is remained to show <a href="#eqn: pispecific" data-reference-type="eqref" data-reference="eqn: pispecific">this equation</a>. One can rewrite <span class="math inline">\(v_{\mu}^{\overline{\pi}}(s, a)\)</span> as <span class="math display">\[\begin{aligned}
            v_{\mu}^{\overline{\pi}}(s, a)&amp;= \mu(s)\overline{\pi}(a|s)+\gamma\sum_{t=0}^{\infty} \gamma^t p_{\mu}^{\pi}(s_{t+1}=s, a_{t+1}=a)\\
            &amp;=\mu(s)+\gamma\sum_{t=0}^{\infty} \gamma^t \sum_{s&#39;, a&#39;}p_{\mu}^{\pi}(s_t=s&#39;, a_t=a&#39;) p(s|s&#39;, a&#39;) \overline{\pi}(a|s)
            \\&amp;= \mu(s)\overline{\pi}(a|s)+\gamma\overline{\pi}(a|s) \sum_{s&#39;, a&#39;}  p(s|s&#39;, a&#39;)\sum_{t=0}^{\infty} \gamma^t p_{\mu}^{\pi}(s_t=s&#39;, a_t=a&#39;)
            \\&amp;= \mu(s)\overline{\pi}(a|s)+\gamma \overline{\pi}(a|s)\sum_{s&#39;, a&#39;} p(s|s&#39;, a&#39;)v_{\mu}^{\overline{\pi}}(s&#39;, a&#39;),
         \end{aligned}\]</span> <a name="eqn: occupancy_bellman"></a>which shows that the occupancy measure is a solution of <span class="math inline">\(\overline{\pi}\)</span>-specific Bellman’s flow constraints.</p>
<p>You might then wonder why don’t we have more solutions? The idea is simple. One can rewrite <span class="math inline">\(\overline{\pi}\)</span>-specific Bellman’s flow equality as <span class="math inline">\(Ax=b\)</span> where <span class="math inline">\(A\)</span> has elements <span class="math display">\[\begin{aligned}
            A_{sa, s&#39;a&#39;}=\left\{\begin{array}{c c}
                1-\gamma \overline{\pi}(a|s)p(s|s&#39; a&#39;) &amp; s&#39;a&#39;=sa,\\
                -\gamma \overline{\pi}(a|s)p(s|s&#39; a&#39;) &amp; o.w.
            \end{array}\right.
         \end{aligned}\]</span> Then, one can simply show the absolute sum of non-diagonal elements of the matrix over <span class="math inline">\((s, a)\)</span> is less than the diagonal element of the column. Then, we can use <a href="https://en.wikipedia.org/wiki/Gershgorin_circle_theorem"> Gershgorin’s circle theorem</a>, which explains the eigenvalues of a matrix are inside a circle with the center of diagonal elements and radius of a sum of non-diagonal elements. Then we can see that matrix <span class="math inline">\(A\)</span> does not have any zero-valued eigenvalue, and hence, it is non-singular and has at most one solution.</p></li>
<li><p>Using the previous section, we know that <span class="math inline">\(v_{\mu}^{\overline{\pi}}\)</span> satisfies <span class="math inline">\(\overline{\pi}\)</span>-specific Bellman’s flow constraints, and consequently the general form of Bellman’s flow constraints which can be written as <span class="math display">\[\begin{aligned}
            v_{\mu}^{\overline{\pi}}(s, a) = \mu(s) +\gamma \sum_{s&#39;, a&#39;} p(s|s&#39;, a&#39;) v_{\mu}^{\overline{\pi} }(s&#39;, a&#39;).
         \end{aligned}\]</span> Using this fact and <a href="#eqn: occupancy_bellman" data-reference-type="eqref" data-reference="eqn: occupancy_bellman">this equation</a>, one can prove the second step.</p></li>
<li><p>Similar to the first step, we rewrite the occupancy measure and show the statement <span class="math display">\[\begin{aligned}
            \sum_{a} v_{\mu}^{\pi}(s, a)&amp;=\sum_{t=0}^{\infty} \gamma^t p_{\mu}^{\pi}(s_t = s)\\&amp; = \mu(s)+\gamma \sum_{t=0}^{\infty} \gamma^t \sum_{s&#39;, a&#39;}p_{\mu}^{\pi}(s_{t}=s&#39;, a_{t}=a&#39;)p(s|s&#39;, a&#39;)\\
            &amp;=\mu(s)+\gamma\sum_{a&#39;, s&#39;} v_{\mu}^{\pi}(s&#39;, a&#39;)p(s|s&#39;, a&#39;).
         \end{aligned}\]</span> Note that in the first equality <span class="math inline">\(p_{\mu}^{\pi}(s_t=s)=\sum_{a} p_{\mu}^{\pi}(s_t=s, a_t=a)\)</span>, and the second equality is true, because <span class="math inline">\(p_{\mu}^{\pi}(s_0=s) =\mu(s)\)</span> and <span class="math display">\[p_{\mu}^{\pi}(s_{t+1}=s, a_{t+1}=a) = \sum_{s&#39;, a&#39;}p_{\mu}^{\pi}(s_{t}=s&#39;, a_{t}=a&#39;)p(s|s&#39;, a&#39;).\]</span></p></li>
<li><p>Since</p>
<p><span class="math display">\[\begin{aligned}
 \overline{\pi}(a|s)= \frac{x(s, a)}{\sum_{a} x(s, a)} = \frac{x(s, a)}{\mu(s)+\gamma \sum_{s&#39;, a&#39;} x (s&#39;, a&#39;) p (s|s&#39;, a&#39;)}, \end{aligned}\]</span></p>
<p>then <span class="math inline">\(x(s, a)\)</span> satisfies <span class="math inline">\(\overline{\pi}\)</span>-specific Bellman flow constraint, which using the first step, <span class="math inline">\(x(s, a)\)</span> is the occupancy measure of <span class="math inline">\(\overline{\pi}\)</span>.</p></li>
<li><p>Using the third step, the occupancy measure satisfies the general form of Bellman’s flow constraints. Then using the fourth step, it is also an occupancy measure of the mentioned stationary policy. Hence, the statement is proved.</p></li>
<h2>Bibliography</h2>

  U.Syed, M. Bowling, and R. E. Schapire, “Apprenticeship learning using linear programming,” in Proceedings of the 25th international conference on Machine learning, 2008, pp. 1032–1039.

</ol>
</body>
</html>
