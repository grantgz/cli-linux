---
title: Perspective
published: true
taxonomy:
    category:
        - docs
---

<html>
<body>
Speed test<br>
In one second you can do...<br>
20655000 function calls<br>
75100 16KB files read from disk (cache)<br>
24800 regular expression replaces over 1KB of text<br>
2900 16KB files written to disk (cache)<br>
3230 get_record calls on the course table<br>
360 insert_record calls on the course table<br>
340 update_record calls on the course table<br>
1.20 seconds to delete 360 entries<br>
Script took " . number_format($time,2) . " seconds to execute.<br>
"; ?><br>
 </body>   
    </html>
    
    
    
    Perspective.php file
    <?php
/**
 * Determines relative performance implications of different types of action.
 *
 * @copyright &copy; 2006 The Open University
 * @author s.marshall@open.ac.uk
 * @license http://www.gnu.org/copyleft/gpl.html GNU Public License
 * @package tools
 *//** */
 
//error_reporting(E_ALL); 

//define('MOODLE_INTERNAL', true);

function microtime_float()
{
    list($usec, $sec) = explode(" ", microtime());
    return ((float)$usec + (float)$sec);
}

$time_start = microtime_float();

 
require_once(dirname(__FILE__).'/config.php');
//require_once(dirname(__FILE__).'/lib/dml/moodle_database.php');

// Approx 1KB text
$longtext='
"But why Turkish?" asked Mr. Sherlock Holmes, gazing fixedly at
my boots.  I was reclining in a cane-backed chair at the moment,
and my protruded feet had attracted his ever-active attention.

"English," I answered in some surprise.  "I got them at
Latimer\'s, in Oxford Street."

Holmes smiled with an expression of weary patience.

"The bath!" he said; "the bath!  Why the relaxing and expensive
Turkish rather than the invigorating home-made article?"

"Because for the last few days I have been feeling rheumatic and
old. A Turkish bath is what we call an alterative in medicine--a
fresh starting-point, a cleanser of the system.

"By the way, Holmes," I added, "I have no doubt the connection
between my boots and a Turkish bath is a perfectly self-evident
one to a logical mind, and yet I should be obliged to you if you
would indicate it."

"The train of reasoning is not very obscure, Watson," said Holmes
with a mischievous twinkle.  "It belongs to the same elementary
class of deduction which I should illustrate if I were to ask you
who shared your cab in your drive this morning."
';
$longtext=substr($longtext,0,1024);

global $count;

function functioncall() {
    global $count;
    $count++;
}

?>
<html>
<head>
<title>Speed test</title>
</head>
<body>
<h1>Speed test</h1>
<h2>In one second you can do...</h2>
<ul>
<?php
flush();

$count=0;
$ready=time();
while(($start=time())==$ready) {
    usleep(1000); 
}
do
{
    for($i=0;$i<1000;$i++) {
        functioncall();
    }
}
while(time()==$start);
print "<li>$count function calls</li>";
flush();

$longertext=$longtext.$longtext.$longtext.$longtext;
$longertext=$longertext.$longertext.$longertext.$longertext;
file_put_contents($CFG->dataroot.'/perspective.temp',$longertext);

$count=0;
$ready=time();
while(($start=time())==$ready) {
    usleep(1000); 
}
do
{
    for($i=0;$i<100;$i++) {
        if(!file_get_contents($CFG->dataroot.'/perspective.temp')) {
            error('Get failed');
        }
        $count++;
    }
}
while(time()==$start);
print "<li>$count 16KB files read from disk (cache)</li>";
flush();

$count=0;
$ready=time();
while(($start=time())==$ready) {
    usleep(1000); 
}
do
{
    for($i=0;$i<100;$i++) {
        preg_replace('/[^c]ie/','',$longtext);
        $count++;
    }
}
while(time()==$start);
print "<li>$count regular expression replaces over 1KB of text</li>";
flush();

$count=0;
$ready=time();
while(($start=time())==$ready) {
    usleep(1000); 
}
do
{
    for($i=0;$i<100;$i++) {
        if(!file_put_contents($CFG->dataroot.'/perspective.temp',$longertext)) {
            error('Put failed');
        }
        $count++;
    }
}
while(time()==$start);
print "<li>$count 16KB files written to disk (cache)</li>";
flush();

$CFG->cachegetrecord=false;


$count=0;
$ready=time();
while(($start=time())==$ready) {
    usleep(1000); 
}
do
{
    for($i=0;$i<10;$i++) {
        if(!$DB->get_record('course',array('id' => 1))) {
            error('Get failed: '.$DB->ErrorMsg());
        }
        $count++;
    }
}
while(time()==$start);
print "<li>$count get_record calls on the course table</li>";
flush();

$newrecord=new StdClass;
$newrecord->shortname='!!!TEST';
$newrecord->fullname='!!!TEST';
$newrecord->sortorder=0;
$ids=array();

$count=0;
$ready=time();
while(($start=time())==$ready) {
    usleep(1000); 
}
do
{
    for($i=0;$i<10;$i++) {
        if(!($ids[]=$DB->insert_record('course',$newrecord))) {
            error('Insert failed: '.$DB->ErrorMsg());
        }
        unset($newrecord->id);
        $newrecord->sortorder++;
        $count++;
    }
}
while(time()==$start);
print "<li>$count insert_record calls on the course table</li>";
flush();


$newrecord=new StdClass;
$newrecord->id=$ids[0];
$newrecord->fullname='TEST!!!';
$count=0;
$ready=time();
while(($start=time())==$ready) {
    usleep(1000); 
}
do
{
    for($i=0;$i<10;$i++) {
        if(!$DB->update_record('course',$newrecord)) {
            error('Update failed: '.$DB->ErrorMsg());
        }
        $count++;
    }
}
while(time()==$start);
print "<li>$count update_record calls on the course table</li>";
flush();

$del_start = microtime_float();
for($count=0;$count<count($ids);$count++) {
	
    if(!$DB->delete_records('course',array('id' => $ids[$count]))) {
        error('Delete failed: '.$DB->ErrorMsg());
    }
}
$del_end = microtime_float();
$time = $del_end - $del_start;
print "<li>" . number_format($time, 2) . " seconds to delete $count entries</li>";

?>
</ul>
<?

$time_end = microtime_float();
$time = $time_end - $time_start;

echo "<p>Script took " . number_format($time,2) . " seconds to execute.</p>";

?>
</body>
</html>
    
    
