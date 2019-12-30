<html><head></head><body><div class="nested0" aria-labelledby="ariaid-title1" topicindex="1" topicid="rbe1506030154959" id="rbe1506030154959"><h1 class="title topictitle1" id="ariaid-title1">Multiple-Input Attribution_MLE (ML Engine)</h1><div class="body conbody">
<p class="p">The multiple-input version of the Attribution_MLE function accepts one or more input tables and gets many parameters from other dimension tables.</p><div class="fig fignone" id="rbe1506030154959__fig_jlj_4rn_lw"><div class="caption"></div><br clear="none"></br><img class="image" id="rbe1506030154959__image_qlr_4rn_lw" src="ovh1466005081880.svg" alt="How the Machine Learning Enginemultiple-input Attribution_MLE function works"></img><br clear="none"></br></div></div><div class="related-links"><div class="linklistheader"><p></p><b>Related Information</b></div>
<ul class="linklist linklist relinfo"><div class="linklistmember"><a href="ppu1541526284885.md#ifq1507592820945">Single-Input Attribution_MLE (ML Engine)</a></div></ul></div><div class="topic reference nested1" aria-labelledby="ariaid-title2" topicindex="2" topicid="uqp1507592291489" xml:lang="en-us" lang="en-us" id="uqp1507592291489">
<h2 class="title topictitle2" id="ariaid-title2">Attribution_MLE Syntax (Multiple Inputs)</h2><div class="body refbody"><div class="section" id="uqp1507592291489__section_N1000E_N1000C_N10001">
<h3 class="title sectiontitle">Version <span>2.10</span></h3><pre class="pre codeblock" xml:space="preserve"><code>SELECT * FROM Attribution_MLE (
  <span>ON { <var class="keyword varname">table</var> | <var class="keyword varname">view</var> | (<var class="keyword varname">query</var>) }</span> PARTITION BY <var class="keyword varname">user_id</var> ORDER BY <var class="keyword varname">timestamp_column</var> 
  [ <span>ON { <var class="keyword varname">table</var> | <var class="keyword varname">view</var> | (<var class="keyword varname">query</var>) }</span> PARTITION BY <var class="keyword varname">user_id</var> ORDER BY <var class="keyword varname">timestamp_column</var> ][,...]
  ON <var class="keyword varname">conversion_event_table</var> AS ConversionEventTable DIMENSION
  [ ON <var class="keyword varname">excluding_event_table</var> AS ExcludedEventTable DIMENSION ]
  [ ON <var class="keyword varname">optional_event_table</var> AS OptionalEventTable DIMENSION ]
  ON <var class="keyword varname">first_model_table</var> AS FirstModelTable DIMENSION
  [ ON <var class="keyword varname">second_model_table</var> AS SecondModelTable DIMENSION ]
  USING
  EventColumn ('<var class="keyword varname">event_column</var>')
  TimeColumn ('<var class="keyword varname">timestamp_column</var>')
  WindowSize ({ 'rows:<var class="keyword varname">K</var>' | 'seconds:<var class="keyword varname">K</var>' | 'rows:<var class="keyword varname">K</var>&amp;seconds:<var class="keyword varname">K2</var>' })
) AS <var class="keyword varname">alias</var> ORDER BY <var class="keyword varname">user_id</var>,<var class="keyword varname">time_stamp</var>;</code></pre></div></div><div class="related-links"><div class="linklistheader"><p></p><b>Related Information</b></div>
<ul class="linklist linklist relinfo"><div class="linklistmember"><a href="eta1543514041091.md">Comments in Queries</a></div></ul></div></div><div class="topic reference nested1" aria-labelledby="ariaid-title3" topicindex="3" topicid="nwj1507592418107" xml:lang="en-us" lang="en-us" id="nwj1507592418107">
<h2 class="title topictitle2" id="ariaid-title3">Attribution_MLE Syntax Elements (Multiple Inputs)</h2><div class="body refbody"><div class="section" id="nwj1507592418107__section_N10011_N1000E_N10001"><dl class="dl parml"><dt class="dt pt dlterm">EventColumn</dt><dd class="dd pd">Specify the name of the input column that contains the clickstream events.</dd><dt class="dt pt dlterm">TimeColumn</dt><dd class="dd pd">Specify the name of the input column that contains the timestamps of the clickstream events.</dd><dt class="dt pt dlterm">WindowSize</dt><dd class="dd pd">Specify how to determine the maximum window size for the attribution calculation:
<div class="tablenoborder"><table cellpadding="4" cellspacing="0" summary="" id="nwj1507592418107__table_a4k_nyx_fdb" class="table" frame="border" border="1" rules="all"><div class="caption"></div><colgroup span="1"><col style="width:50%" span="1"></col><col style="width:50%" span="1"></col></colgroup><thead class="thead" style="text-align:left;"><tr class="row"><th class="entry cellrowborder" style="vertical-align:top;" id="d72766e203" rowspan="1" colspan="1">Option</th><th class="entry cellrowborder" style="vertical-align:top;" id="d72766e205" rowspan="1" colspan="1">Description</th></tr></thead><tbody class="tbody"><tr class="row"><td class="entry cellrowborder" style="vertical-align:top;" headers="d72766e203" rowspan="1" colspan="1"><code class="ph codeph">rows:</code><var class="keyword varname">K</var></td><td class="entry cellrowborder" style="vertical-align:top;" headers="d72766e205" rowspan="1" colspan="1">Assign attributions to at most <var class="keyword varname">K</var> events before conversion event, excluding events of types specified in ExcludedEventTable.</td></tr><tr class="row"><td class="entry cellrowborder" style="vertical-align:top;" headers="d72766e203" rowspan="1" colspan="1"><code class="ph codeph">seconds:</code><var class="keyword varname">K</var></td><td class="entry cellrowborder" style="vertical-align:top;" headers="d72766e205" rowspan="1" colspan="1">Assign attributions only to rows not more than <var class="keyword varname">K</var> seconds before conversion event.</td></tr><tr class="row"><td class="entry cellrowborder" style="vertical-align:top;" headers="d72766e203" rowspan="1" colspan="1"><code class="ph codeph">rows:</code><var class="keyword varname">K</var><code class="ph codeph">&amp;seconds:</code><var class="keyword varname">K2</var></td><td class="entry cellrowborder" style="vertical-align:top;" headers="d72766e205" rowspan="1" colspan="1">Apply both constraints and comply with stricter one.</td></tr></tbody></table></div></dd></dl></div></div></div></div></body></html>