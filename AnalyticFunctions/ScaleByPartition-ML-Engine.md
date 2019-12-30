<html><head></head><body><div class="nested0" aria-labelledby="ariaid-title1" topicindex="1" topicid="fcl1507824491377" id="fcl1507824491377"><h1 class="title topictitle1" id="ariaid-title1">ScaleByPartition (ML Engine)</h1><div class="body conbody">
<p class="p">The ScaleByPartition function scales the sequences in each partition independently, using the same formula as the function <a href="hal1558454832381.md#zzl1507825806540">Scale (ML Engine)</a>.</p></div><div class="topic reference nested1" aria-labelledby="ariaid-title2" topicindex="2" topicid="qfp1507824518685" xml:lang="en-us" lang="en-us" id="qfp1507824518685">
<h2 class="title topictitle2" id="ariaid-title2">ScaleByPartition Syntax</h2><div class="body refbody"><div class="section" id="qfp1507824518685__section_N1000E_N1000C_N10001">
<h3 class="title sectiontitle">Version <span>1.7</span></h3><pre class="pre codeblock" xml:space="preserve"><code>SELECT * FROM ScaleByPartition (
  <span>ON { <var class="keyword varname">table</var> | <var class="keyword varname">view</var> | (<var class="keyword varname">query</var>) }</span> PARTITION BY <var class="keyword varname">partition_columns</var>
  USING
  ScaleMethod ('<var class="keyword varname">method</var>' [,…])
  [ MissValue ({ 'KEEP' | 'OMIT' | 'ZERO' | 'LOCATION' })]
  TargetColumns ( { '<var class="keyword varname">target_column</var>' | <var class="keyword varname">target_column_range</var> }[,...] )
  [ GlobalScale (<span><b>{'true'|'t'|'yes'|'y'|'1'|'false'|'f'|'no'|'n'|'0'}</b></span>) ]
  <code class="ph codeph">[ Accumulate ({ '<var class="keyword varname">accumulate_column</var>' | <var class="keyword varname">accumulate_column_range</var> }[,...]) ]</code>
  [ Multiplier (<var class="keyword varname">multiplier</var> [,...]) ]
  [ Intercept (<var class="keyword varname">intercept</var> [,...]) ]
) AS <var class="keyword varname">alias</var>;</code></pre></div></div><div class="related-links"><div class="linklistheader"><p></p><b>Related Information</b></div>
<ul class="linklist linklist relinfo"><div class="linklistmember"><a href="ndv1557782188375.md">Column Specification Syntax Elements</a></div></ul></div></div><div class="topic reference nested1" aria-labelledby="ariaid-title3" topicindex="3" topicid="hvx1507824522770" xml:lang="en-us" lang="en-us" id="hvx1507824522770">
<h2 class="title topictitle2" id="ariaid-title3">ScaleByPartition Syntax Elements</h2><div class="body refbody"><div class="section" id="hvx1507824522770__section_N10011_N1000E_N10001"><dl class="dl parml"><dt class="dt pt dlterm">ScaleMethod</dt><dd class="dd pd">Specify one or more statistical methods to use to scale the data set. For <var class="keyword varname">method</var> values and descriptions, see <a href="hal1558454832381.md#pxc1522254582043">Location and Scale for Statistical Methods</a>.
<p class="p">If you specify multiple methods, the output table includes the column scalemethod (which contains the method name) and a row for each input-row/method combination.</p></dd><dt class="dt pt dlterm">MissValue</dt><dd class="dd pd">[Optional] Specify how the ScaleByPartition function is to process NULL values in input:
<div class="tablenoborder"><table cellpadding="4" cellspacing="0" summary="" id="hvx1507824522770__table_ndp_vv2_gdb" class="table" frame="border" border="1" rules="all"><div class="caption"></div><colgroup span="1"><col style="width:50%" span="1"></col><col style="width:50%" span="1"></col></colgroup><thead class="thead" style="text-align:left;"><tr class="row"><th class="entry cellrowborder" style="vertical-align:top;" id="d95324e170" rowspan="1" colspan="1">Option</th><th class="entry cellrowborder" style="vertical-align:top;" id="d95324e172" rowspan="1" colspan="1">Description</th></tr></thead><tbody class="tbody"><tr class="row"><td class="entry cellrowborder" style="vertical-align:top;" headers="d95324e170" rowspan="1" colspan="1"><code class="ph codeph">KEEP</code> (Default)</td><td class="entry cellrowborder" style="vertical-align:top;" headers="d95324e172" rowspan="1" colspan="1">Keep NULL values.</td></tr><tr class="row"><td class="entry cellrowborder" style="vertical-align:top;" headers="d95324e170" rowspan="1" colspan="1"><code class="ph codeph">OMIT</code></td><td class="entry cellrowborder" style="vertical-align:top;" headers="d95324e172" rowspan="1" colspan="1">Ignore any row that has a NULL value.</td></tr><tr class="row"><td class="entry cellrowborder" style="vertical-align:top;" headers="d95324e170" rowspan="1" colspan="1"><code class="ph codeph">ZERO</code></td><td class="entry cellrowborder" style="vertical-align:top;" headers="d95324e172" rowspan="1" colspan="1">Replace each NULL value with zero.</td></tr><tr class="row"><td class="entry cellrowborder" style="vertical-align:top;" headers="d95324e170" rowspan="1" colspan="1"><code class="ph codeph">LOCATION</code></td><td class="entry cellrowborder" style="vertical-align:top;" headers="d95324e172" rowspan="1" colspan="1">Replace each NULL value with its location value.</td></tr></tbody></table></div></dd><dt class="dt pt dlterm">TargetColumns</dt><dd class="dd pd">Specify the input table columns that contain the values to scale. The columns must contain numeric values must between -1e<span><sup>308</sup></span> and 1e<span><sup>308</sup></span>. If a value is outside this range, the function treats it as infinity.</dd><dt class="dt pt dlterm">GlobalScale</dt><dd class="dd pd">[Optional] Specify whether all input columns are scaled to the same location and scale.</dd><dd class="dd pd ddexpand">Default: 'false' (Each input column is scaled separately.)</dd><dt class="dt pt dlterm">Accumulate</dt><dd class="dd pd">[Optional] Specify the input table columns to copy to the output table.<div class="note tip" id="hvx1507824522770__note_N100B3_N100AE_N100A1_N1003D_N10015_N10011_N1000E_N10001"><span><b>Tip</b></span><div class="notebody">To identify the sequences in the output, specify the partition columns in this syntax element.</div></div></dd><dt class="dt pt dlterm">Multiplier</dt><dd class="dd pd">[Optional] Specify one or more multiplying factors to apply to the input variables (<var class="keyword varname">multiplier</var> in this formula):
<p class="p"><var class="keyword varname">X'</var> = <var class="keyword varname">intercept</var> + <var class="keyword varname">multiplier</var> * (<var class="keyword varname">X</var> - <var class="keyword varname">location</var>)/<var class="keyword varname">scale</var></p></dd><dd class="dd pd ddexpand">If you specify only one <var class="keyword varname">multiplier</var>, it applies to all columns specified by the TargetColumns syntax element.</dd><dd class="dd pd ddexpand">If you specify multiple multiplying factors, each <var class="keyword varname">multiplier</var> applies to the corresponding input column. For example, the first <var class="keyword varname">multiplier</var> applies to the first column specified by the TargetColumns syntax element, the second <var class="keyword varname">multiplier</var> applies to the second input column, and so on.</dd><dd class="dd pd ddexpand">Default: <var class="keyword varname">multiplier</var> is 1.</dd><dt class="dt pt dlterm">Intercept</dt><dd class="dd pd">[Optional] Specify one or more addition factors incrementing the scaled results—<var class="keyword varname">intercept</var> in this formula:
<p class="p"><var class="keyword varname">X'</var> = <var class="keyword varname">intercept</var> + <var class="keyword varname">multiplier</var> * (<var class="keyword varname">X</var> - <var class="keyword varname">location</var>)/<var class="keyword varname">scale</var></p></dd><dd class="dd pd ddexpand">If you specify only one <var class="keyword varname">intercept</var>, it applies to all columns specified by the TargetColumns syntax element. If you specify multiple addition factors, each <var class="keyword varname">intercept</var> applies to the corresponding input column.</dd><dd class="dd pd ddexpand">This is the syntax of <var class="keyword varname">intercept</var>:<pre class="pre codeblock" xml:space="preserve"><code>[-]{<var class="keyword varname">number</var> | min | mean | max }</code></pre></dd><dd class="dd pd ddexpand">where <code class="ph codeph">min</code>, <code class="ph codeph">mean</code>, and <code class="ph codeph">max</code> are the scaled global minimum, maximum, mean values of the corresponding columns.</dd><dd class="dd pd ddexpand">This is the formula for computing the scaled global minimum:
<p class="p"><var class="keyword varname">scaledmin</var> = (minX - <var class="keyword varname">location</var>)/<var class="keyword varname">scale</var></p></dd><dd class="dd pd ddexpand">The formulas for computing the scaled global mean and maximum are analogous to the preceding formula. For example, if <var class="keyword varname">intercept</var> is <code class="ph codeph">'- min'</code> and <var class="keyword varname">multiplier</var> is 1, this is the formula for computing the scaled result X':
<p class="p">X' = -<var class="keyword varname">scaledmin</var> + 1 * (X - <var class="keyword varname">location</var>/<var class="keyword varname">scale</var>)</p></dd><dd class="dd pd ddexpand">Default: <var class="keyword varname">intercept</var> is 0.</dd></dl></div></div></div></div></body></html>