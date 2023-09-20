# woodmart-core-plugin-svg-error

```
if (!function_exists('woodmart_get_svg')) {
    function woodmart_get_svg($file)
    {
        if (!apply_filters('woodmart_svg_cache', true)) {
            return ''; // Return an empty string or handle the error as needed.
        }

        // Check if the file exists before attempting to read its contents.
        if (file_exists($file)) {
            $file_path = array_reverse(explode('/', $file));
            $slug = 'wdm-svg-' . $file_path[2] . '-' . $file_path[1] . '-' . $file_path[0];
            $content = get_transient($slug);

            if (!$content) {
                $file_get_contents = file_get_contents($file);

                if (strstr($file_get_contents, '<svg')) {
                    $content = woodmart_compress($file_get_contents);
                    set_transient($slug, $content, apply_filters('woodmart_svg_cache_time', 60 * 60 * 24 * 7));
                }
            }

            return woodmart_decompress($content);
        } else {
            // Handle the case where the file does not exist.
            return ''; // Return an empty string or handle the error as needed.
        }
    }
}
```
