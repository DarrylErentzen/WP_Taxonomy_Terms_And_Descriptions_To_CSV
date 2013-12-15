<?php 
$path = "wherever/wp-load.php/is";
$output_path = '/full/system/path/to/output/folder'; // NOTE: make sure permissions are correct to allow web server to write to folder, 755 should do it.

include($path.'/wp-load.php');
//get all taxonomies and store slugs in array
$taxes = get_taxonomies();

// loop through taxonomies
foreach($taxes as $tax_name) {
	$csv_output = ''; // create variable to hold output
	
	$args = array('parent' => 0);
	$terms = get_terms ($tax_name, $args);//get parent terms
	
	foreach($terms as $term) {//loop through parent terms
		$args = array('parent' => $term->term_id);
		$child_terms = get_terms ($tax_name, $args);//get child terms
		if(count($child_terms) > 0) {// if $child_terms isn't empty
			foreach($child_terms as $child_term) {// loop
				// output terms in csv-format "parent,child,description\n" to file
				$csv_output .= '"'.$term->name.'","'.$child_term->name.'","'.$child_term->description.'"'."\n";
			} //end child term loop
		} //end not empty
	}//end parent term loop
	
	//write output file
	$file = $output_path.'/'.$tax_name.'.txt';
	append($file,$csv_output);
	
} //end taxonomy loop

function append($file, $data, $mode = 'a')
{
    // Ensure $file exists. Just opening it with 'w' or 'a' might cause
    // 1 process to clobber another's.
    $fp = @fopen($file, 'x');
    if ($fp)
        fclose($fp);

    // Append
    $lock = strlen($data) > 4096; // assume PIPE_BUF is 4096 (Linux)

    $fp = fopen($file, $mode);
    if ($lock && !flock($fp, LOCK_EX))
        throw new Exception('Cannot lock file: '.$file);
    fwrite($fp, $data);
    if ($lock)
        flock($fp, LOCK_UN);
    fclose($fp);
}
			

?>
