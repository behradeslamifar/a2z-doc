# Synergy
synergys.conf

```
# sample synergy configuration file
#
# comments begin with the # character and continue to the end of
# line.  comments may appear anywhere the syntax permits.

section: screens
	# three hosts named:  moe, larry, and curly
	laptop:
	dunro:
end

section: links
	# larry is to the right of moe and curly is above moe
	laptop:
		left = dunro

	# moe is to the left of larry and curly is above larry.
	# note that curly is above both moe and larry and moe
	# and larry have a symmetric connection (they're in
	# opposite directions of each other).
	dunro:
		right = laptop
end

#section: aliases
#	# curly is also known as shemp
#	curly:
#		shemp
#end
section: options

	keystroke(Alt+Left) = switchToScreen(dunro)
	keystroke(Alt+Right) = switchToScreen(laptop)
end
```
