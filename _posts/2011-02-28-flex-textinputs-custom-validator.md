---
layout: post
header-img: img/default-blog-pic.jpg
---

# Flex: Validator versus TextInput's restrict

I came across a requirement where i need to restrict user from entering numeric values. The first solution that came into my mind was to simply restrict the input textfield, by using TextInput's restrict method. During the sprint demo, a small suggestion came (from a usability perspective) - _Instead of restricting user, can't i alert him with some validation errors?_. It was a nice thought, as user can be shown with the reasons of this error, which is not possible if we simply 'restrict' the user.  So after a small R&D, i was able to write a validator for the same. [Attached](/wp-content/uploads/2011/03/ValidateAlphabets.zip) is the code. The end result was - The textfield will show validation errors if there are any numeric values found. The main trick here was to override the 'doValidation()' method. The highlights of this method are: 

  * Override this method in your custom validator class
  * Contains the logic for validation.
  * The result of validation is stored in _validationResults, which accepts objects of type ValidationResult.
  * ValidationResult contains the error message to be displayed in the tooltip.
[code] override protected function doValidation(value:Object):Array { var trimmedValue:String = StringUtil.trim(String(value)); _validationResults = super.doValidation(trimmedValue); if(_validationResults.length > 0 || trimmedValue.length == 0) return _validationResults; if(!CharacterUtil.validateCharacters(DECIMAL_DIGITS, trimmedValue)) { _validationResults.push(new ValidationResult(true, null, ApplicationConstants.INVALID_CHARACTER, _messageForInvalidCharacter)); return _validationResults; } else { Alert.show("No validation errors found", "Alert"); } return _validationResults; } [/code] CharacterUtil is the class which encapsulates the logic to check for the numeric values in the entered characters. If you want to write your custom validator, just change the method written inside this class, with your checks and conditions.

## Comments

**[Gaurav Srivastava](#5575 "2011-05-16 14:26:06"):** Agree. But as per the requirements, we had many custom validation rules. So in all cases, we have to override doValidation() to write any custom implementation.

**[Vijay Mareddy](#5567 "2011-05-11 01:07:44"):** You could use the NumberValidator which has a validateNumber function

