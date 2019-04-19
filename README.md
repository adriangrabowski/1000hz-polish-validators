# [1000hz-bootstrap-validator] Usefull code for polish validation of: NIP, PESEL, REGON, IDENTITY CARD NUMBER
This code is pack of custom validators for polish data types: NIP, PESEL, REGON and IDENTITY CARD NUMBER.

### Requirements
* jQuery https://jquery.com
* Dedicated for https://github.com/1000hz/bootstrap-validator 

### Usage
In your bootstrap-validator initialization:
```js
$('form[data-toggle="validator"]').validator({
    custom: {
	// [...],
	'pesel': function($el) {
		var weight = [1, 3, 7, 9, 1, 3, 7, 9, 1, 3];
		var sum = 0;
		var controlNumber = parseInt($el.val().substring(10, 11));
		for (var i = 0; i < weight.length; i++) {
			sum += (parseInt($el.val().substring(i, i + 1)) * weight[i]);
		}
		sum = sum % 10;
		return !(10 - sum === controlNumber);
	},
	'identity-card-number': function($el) {
		var number = $el.val();

		if (number == null || number.length != 9)
			return true;

		number = number.toUpperCase();
		var letterValues = '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ';
		function getLetterValue(letter){
			return letterValues.indexOf(letter)
		}

		for (i=0; i<3; i++)
			if (getLetterValue(number[i]) < 10 || number[i] == 'O' || number == 'Q')
				return true;
		for (i=3; i<9; i++)
			if (getLetterValue(number[i]) < 0 || getLetterValue(number[i]) > 9)
				return true;

		var sum = 7 * getLetterValue(number[0]) +
			3 * getLetterValue(number[1]) +
			1 * getLetterValue(number[2]) +
			7 * getLetterValue(number[4]) +
			3 * getLetterValue(number[5]) +
			1 * getLetterValue(number[6]) +
			7 * getLetterValue(number[7]) +
			3 * getLetterValue(number[8]);
		sum %= 10;

		return sum != getLetterValue(number[3]);
	},
	'nip': function($el) {
		var nip = $el.val();

		if(typeof nip !== 'string')
			return false;

		nip = nip.replace(/[\ \-]/gi, '');

		var weight = [6, 5, 7, 2, 3, 4, 5, 6, 7];
		var sum = 0;
		var controlNumber = parseInt(nip.substring(9, 10));
		for (var i = 0; i < weight.length; i++) {
			sum += (parseInt(nip.substring(i, i + 1)) * weight[i]);
		}

		return !(sum % 11 === controlNumber);
	},
	'regon': function($el) {
		var regon = $el.val();

		var reg = /^[0-9]{9}$/;
		if (!reg.test(regon)) {
			return true;
		} else {
			var digits = (""+regon).split("");
			var checksum = (8*parseInt(digits[0]) + 9*parseInt(digits[1]) + 2*parseInt(digits[2]) + 3*parseInt(digits[3]) + 4*parseInt(digits[4]) + 5*parseInt(digits[5]) + 6*parseInt(digits[6]) + 7*parseInt(digits[7]))%11;
			if(checksum == 10)
				checksum = 0;

			return !(parseInt(digits[8])==checksum);
		}
	},
	// [...]
    }
});

```
### Examples
In your HTML template (example for PESEL): 
```html
<div class="row">
    <div class="form-group col-xs-12 col-md-12">
        <label class="control-label" for="test">PESEL:</label>
        <input type="text" name="test" id="test" class="form-control" placeholder="Enter PESEL" data-pesel="pesel" data-pesel-error="This is not valid PESEL number">
        <span class="help-block with-errors"></span>
    </div>
</div>
```

In your HTML template (example for IDENTITY CARD NUMBER): 
```html
<div class="row">
    <div class="form-group col-xs-12 col-md-12">
        <label class="control-label" for="test">Identity card number:</label>
        <input type="text" name="test" id="test" class="form-control" placeholder="Enter identity card number" data-identity-card-number="identity-card-number" data-identity-card-number-error="This is not valid identity card number">
        <span class="help-block with-errors"></span>
    </div>
</div>
```
In your HTML template (example for NIP): 
```html
<div class="row">
    <div class="form-group col-xs-12 col-md-12">
        <label class="control-label" for="test">NIP:</label>
        <input type="text" name="test" id="test" class="form-control" placeholder="Enter NIP" data-nip="nip" data-nip-error="This is not valid NIP number">
        <span class="help-block with-errors"></span>
    </div>
</div>
```

In your HTML template (example for REGON): 
```html
<div class="row">
    <div class="form-group col-xs-12 col-md-12">
        <label class="control-label" for="test">REGON:</label>
        <input type="text" name="test" id="test" class="form-control" placeholder="Enter REGON" data-regon="regon" data-regon-error="This is not valid REGON number">
        <span class="help-block with-errors"></span>
    </div>
</div>
```
