# MazeMetal
Indeterminate music. Scipting language and web audio engine in javascript

# Example JSON 

```javascript
{
	"title": "Litany of Regrets",
	"mp3": "litanyofregrets.mp3",
	"artist": "Krallice",
	"total_length": 819.0,
	"sections": {
		"most_of_the_song": {"start": 0, "duration": 663.958},
		"end_1": {"duration": 4.622, "start": 663.958}, 
		"end_2": {"duration": 4.545, "start": 668.58},
		"end_3": {"duration": 4.576, "start": 673.125},
		"end_4": {"duration": 4.491, "start": 677.701},
		"end_5": {"duration": 4.486, "start": 682.192}, 
		"end_6": {"duration": 4.438, "start": 686.678},
		"end_7": {"duration": 4.366, "start": 691.116}, 
		"end_exit": {"duration": 4.249, "start": 695.482},
		"rest_of_the_song": {"duration": 119.269, "start": 699.731}
	},
	"version": "mazemetal0.01", 
	"machine": {
		"main": "
		  label L1
			  play end_1
			label L2
			  play end_2
			  goto L1 0.5
			label L3
			  play end_3
			  goto L2 0.5
			label L4
			  play end_4
			  goto L3 0.5
			label L5
			  play end_5
			  goto L4 0.5
			label L6
			  play end_6
			  goto L5 0.5 
			f end7repeat
			play end_exit
			play rest_of_the_song
			label return
		",

		"end7repeat": "
			label end7
			play end_7
			goto end7 0.9
		"
	}
}
```


* main is the primary f that runs. 
* An f is a series of commands separated by semicolon. 
* They execute top to bottom.
* When main gets to the bottom, the machine and the music terminates.

### Syntax


    Commands:
      label label_name
      	name curent location in script, so we can jump to it.
      
      goto label_name
        jump to label
      goto label_name 0.3
        jump to label 30% of the time (else continue 70% of the time)
      goto L1 0.5 L2 0.5
        jump to L1 half the time, jump to L2 half the time 
      goto A 0.1 B 0.15
        jump to A 10% of the time, jump to B 15% of the time (else continue 75% of the time)
      goto X 0.6, Y 0.6, Z 0.6
        error: sum of probabilities > 1.0
      
      play section_name
        play the audio (whose start and duration are defined above inside "sections"), continue when completed
      
      f script_name
        jump to the subroutine (defined inside "machine"); when completed jump back here 
      
      terminate
        end the music, end the machine
      
            
      * label names and f names must be only: [A-Za-z0-9_] 
      * Everything is case-insensitive. 

## Advanced stuff----

### Stack Overflow
	* There is a subroutine stack. 
	* Calling f adds a subroutine to the stack. 
	* Exiting a subroutine pops it from the stack. 
	* If you go too deeply you may get "stack overflow".

### Label Scope 
	* labels are scoped only within the subroutine they were defined.

### f main
	* It is possible to call "f main", and for a subroutine to call itself. Warning: may cause stackoverflow 

