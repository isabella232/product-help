<html><head></head><body><div class="nested0" aria-labelledby="ariaid-title1" topicindex="1" topicid="ppf1534196263854" id="ppf1534196263854"><h1 class="title topictitle1" id="ariaid-title1">nPath® (ML Engine)</h1><div class="topic reference nested1" aria-labelledby="ariaid-title2" topicindex="2" topicid="ize1507231436584" xml:lang="en-us" lang="en-us" id="ize1507231436584">
<h2 class="title topictitle2" id="ariaid-title2">nPath Syntax</h2><div class="body refbody"><div class="section" id="ize1507231436584__section_N1000E_N1000C_N10001">
<h3 class="title sectiontitle">Version 1.1</h3><div class="note note" id="ize1507231436584__note_N10019_N10011_N1000E_N10001"><span><b>Note</b></span><div class="notebody">This function requires "@coprocessor".</div></div><pre class="pre codeblock" xml:space="preserve"><code>SELECT * FROM nPath@coprocessor (
  <span>ON { <var class="keyword varname">table</var> | <var class="keyword varname">view</var> | (<var class="keyword varname">query</var>) }</span> PARTITION BY <var class="keyword varname">partition_column</var> ORDER BY <var class="keyword varname">order_column</var> [ ASC | DESC ][<var class="keyword varname">...</var>]
  [ <span>ON { <var class="keyword varname">table</var> | <var class="keyword varname">view</var> | (<var class="keyword varname">query</var>) }</span> 
    [ PARTITION BY <var class="keyword varname">partition_column</var> | DIMENSION ] ORDER BY <var class="keyword varname">order_column</var> [ ASC | DESC ] 
  ][<var class="keyword varname">...</var>]
  USING 
  Mode ({ OVERLAPPING | NONOVERLAPPING })
  Pattern ('<var class="keyword varname">pattern</var>')
  Symbols ({ <var class="keyword varname">col_expr</var> = <var class="keyword varname">symbol_predicate</var> AS <var class="keyword varname">symbol</var> } [,...])
  [ Filter (<var class="keyword varname">filter_expression</var> [,...]) ]
  Result ({ <var class="keyword varname">aggregate_function</var> (<var class="keyword varname">col_expr</var> OF <var class="keyword varname">symbol</var>) AS <var class="keyword varname">alias_1</var> }[,...])
) AS <var class="keyword varname">alias_2</var>;</code></pre>
<p class="p">The nPath function is not tied to any schema and must not be qualified with a schema name.</p></div></div><div class="related-links"><div class="linklistheader"><p></p><b>Related Information</b></div>
<ul class="linklist linklist relinfo"><div class="linklistmember"><a href="eta1543514041091.md">Comments in Queries</a></div></ul></div></div><div class="topic reference nested1" aria-labelledby="ariaid-title3" topicindex="3" topicid="sal1507231621140" xml:lang="en-us" lang="en-us" id="sal1507231621140">
<h2 class="title topictitle2" id="ariaid-title3">nPath Syntax Elements</h2><div class="body refbody"><div class="section" id="sal1507231621140__section_N10011_N1000E_N10001"><dl class="dl parml"><dt class="dt pt dlterm">Mode</dt><dd class="dd pd">Specify the pattern-matching mode:
<div class="tablenoborder"><table cellpadding="4" cellspacing="0" summary="" id="sal1507231621140__table_sng_gmy_fdb" class="table" frame="border" border="1" rules="all"><div class="caption"></div><colgroup span="1"><col style="width:50%" span="1"></col><col style="width:50%" span="1"></col></colgroup><thead class="thead" style="text-align:left;"><tr class="row"><th class="entry cellrowborder" style="vertical-align:top;" id="d20892e157" rowspan="1" colspan="1">Option</th><th class="entry cellrowborder" style="vertical-align:top;" id="d20892e159" rowspan="1" colspan="1">Description</th></tr></thead><tbody class="tbody"><tr class="row"><td class="entry cellrowborder" style="vertical-align:top;" headers="d20892e157" rowspan="1" colspan="1"><code class="ph codeph">OVERLAPPING</code></td><td class="entry cellrowborder" style="vertical-align:top;" headers="d20892e159" rowspan="1" colspan="1">Find every occurrence of pattern in partition, regardless of whether it is part of a previously found match. One row can match multiple symbols in a given matched pattern.</td></tr><tr class="row"><td class="entry cellrowborder" style="vertical-align:top;" headers="d20892e157" rowspan="1" colspan="1"><code class="ph codeph">NONOVERLAPPING</code></td><td class="entry cellrowborder" style="vertical-align:top;" headers="d20892e159" rowspan="1" colspan="1">Start next pattern search at row that follows last pattern match.</td></tr></tbody></table></div></dd><dt class="dt pt dlterm">Pattern</dt><dd class="dd pd">Specify the pattern for which the function searches. You compose <var class="keyword varname">pattern</var> with the symbols (which you define in the Symbols syntax element), operators, and parentheses. 
<p class="p">When patterns have multiple operators, the function applies them in order of precedence, and applies operators of equal precedence from left to right. To force the function to evaluate a subpattern first, enclose it in parentheses. For more information, see <a href="upd1562015425006.md">nPath Patterns</a>.</p></dd><dt class="dt pt dlterm">Symbols</dt><dd class="dd pd">Specify the symbols that appear in the values of the Pattern and Result syntax elements. The <var class="keyword varname">col_expr</var> is an expression whose value is a column name, <var class="keyword varname">symbol</var> is any valid identifier, and <var class="keyword varname">symbol_predicate</var> is a SQL predicate (often a column name).
<p class="p">For example, this Symbols syntax element is for analyzing website visits:</p><pre class="pre codeblock" xml:space="preserve"><code>Symbols (
  pagetype = 'homepage' AS H,
  pagetype <> 'homepage' AND pagetype <> 'checkout' AS PP,
  pagetype = 'checkout' AS CO
)</code></pre>
<p class="p">The <var class="keyword varname">symbol</var> is case-insensitive; however, a <var class="keyword varname">symbol</var> of one or two uppercase letters is easy to identify in patterns.</p>
<p class="p">If <var class="keyword varname">col_expr</var> represents a column that appears in multiple input tables, you must qualify the ambiguous column name with its table name. For example:</p><pre class="pre codeblock" xml:space="preserve"><code>Symbols (
  weblog.pagetype = 'homepage' AS H,
  weblog.pagetype = 'thankyou' AS T,
  ads.adname = 'xmaspromo' AS X,
  ads.adname = 'realtorpromo' AS R
)</code></pre>
<p class="p">For more information about symbols that appear in the Pattern syntax element value, see <a href="uvz1542212080166.md">nPath Symbols</a>. For more information about symbols that appear in the Result syntax element value, see <a href="zxb1542212584718.md">nPath Results</a>.</p></dd><dt class="dt pt dlterm">Filter</dt><dd class="dd pd">[Optional] Specify filters to impose on the matched rows. The function combines the filter expressions using the AND operator.
<p class="p">This is the <var class="keyword varname">filter_expression</var> syntax:</p><pre class="pre codeblock" xml:space="preserve"><code><var class="keyword varname">symbol_expression comparison_operator symbol_expression</var></code></pre>
<p class="p">The two symbol expressions must be type-compatible. This is the <var class="keyword varname">symbol_expression</var> syntax:</p><pre class="pre codeblock" xml:space="preserve"><code>{ FIRST | LAST }(<var class="keyword varname">column_with_expression</var> OF [ANY](<var class="keyword varname">symbol</var>[,...]))</code></pre>
<p class="p">The <var class="keyword varname">column_with_expression</var> cannot contain the operator AND or OR, and all its columns must come from the same input. If the function has multiple inputs, <var class="keyword varname">column_with_expression</var> and symbol must come from the same input.</p>
<p class="p">The <var class="keyword varname">comparison_operator</var> is either <code class="ph codeph"><</code>, <code class="ph codeph">></code>, <code class="ph codeph"><=</code>, <code class="ph codeph">>=</code>, <code class="ph codeph">=</code>, or <code class="ph codeph">!=</code>.</p>
<p class="p">Whether this syntax element improves or degrades nPath performance depends on several factors. For details, see <a href="kna1562015310391.md">nPath Filters</a>.</p></dd><dt class="dt pt dlterm">Result</dt><dd class="dd pd">Specify the output columns. The <var class="keyword varname">col_expr</var> is an expression whose value is a column name; it specifies the values to retrieve from the matched rows. The function applies <var class="keyword varname">aggregate_function</var> to these values. For details, see <a href="zxb1542212584718.md">nPath Results</a>.
<p class="p">The function evaluates this syntax element once for every matched pattern in the partition (that is, it outputs one row for each pattern match).</p></dd></dl></div></div></div></div></body></html>