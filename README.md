# godot-snippets

**A list of useful Godot snippets.**

## Basics

### Instancing a scene by code

```gdscript
var scene_file = preload("res://path/to/scene_file.tscn")
var scene_instance = scene_file.instance()
add_child(scene_instance)
```

### Play some music

```gdscript
func play(music):
	var stream = load("res://" + str(music) + ".opus")
	get_node("StreamPlayer").set_stream(stream)
	get_node("StreamPlayer").play()
```

### Printing only when running from editor, or debug-mode exports

```gdscript
func print_debug(text):
	if OS.is_debug_build():
		print(text)
```

### Printing to console with a timestamp

```gdscript
# YYYY-MM-DD
func get_date():
	return str(OS.get_date()["year"]) + "-" + str(OS.get_date()["month"]).pad_zeros(2) + "-" + str(OS.get_date()["day"]).pad_zeros(2)

# hh:mm:ss
func get_time():
	return str(OS.get_time()["hour"]).pad_zeros(2) + ":" + str(OS.get_time()["minute"]).pad_zeros(2) + ":" + str(OS.get_time()["second"]).pad_zeros(2)

# [YYYY-MM-DD | hh:mm:ss]
func print_timestamp(text):
	print("[" + get_date() + " | " + get_time() + "] " + text)
```

### Make a string translatable (with possible replacement)

```gdscript
func _ready():
	print(tr("HELLO_WORLD"))
	print(tr("COLLECT_N_APPLES").replace("%s", str(apples)))
```

## Advanced

### Game options management using an .ini file

```gdscript
func get_option(section, key, default):
	var config = ConfigFile.new()
	config.load("user://config.ini")
	var value = config.get_value(section, key, default)
	return value

func set_option(section, key, value):
	var config = ConfigFile.new()
	config.load("user://config.ini")
	config.set_value(section, key, value)
	config.save("user://config.ini")
```

### Drawing a dashed line
```gdscript
func draw_dashed_line(from, to, color, width, dash_length = 5, cap_end = false, antialiased = false):
    var length = (to - from).length()
    var normal = (to - from).normalized()
    var dash_step = normal * dash_length
    
    if length < dash_length: #not long enough to dash
        draw_line(from, to, color, width, antialiased)
        return
    else:
        var draw_flag = true
        var segment_start = from
        var steps = length/dash_length
        for start_length in range(0, steps + 1):
            var segment_end = segment_start + dash_step
            if draw_flag:
                draw_line(segment_start, segment_end, color, width, antialiased)

            segment_start = segment_end
            draw_flag = !draw_flag
        
        if cap_end:
            draw_line(segment_start, to, color, width, antialiased)
```

## License

Copyright (c) 2016 Calinou and contributors  
This file and all snippets included are under CC0 1.0 Universal. See `LICENSE.md` for more information.
