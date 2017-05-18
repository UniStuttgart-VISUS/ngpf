Issue: #6

# Coding Conventions
The suggested coding conventions of the `ngpf` project are based on: http://thelib.sourceforge.net/index.php/StyleGuide

## Changes
### config.h
Currently, we do not use a central configuration header.

### Stack Trace
Currently, we do not use a library-implemented custom stack trace.

### Licence comment
We have to choose a licence, yet.

### Class indentation
Do not indent class declaration after namespace specification.

`example.h`:

	namespace alpha {
	namespace beta {

	class charlie {
	public:
	    charlie(void);

	    ~charlie(void);
	private:
	    float member;
	}; /* end class charlie */

	} /* end namespace beta */
	} /* end namespace alpha */
