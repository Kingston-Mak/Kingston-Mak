- 👋 Hi, I’m @Kingston-Mak
- 👀 I’m interested in... hacks
- 🌱 I’m currently learning... codeing
- 💞️ I’m looking to collaborate.. on hack
- 📫 How to reach me... i do not know
                                         
and yea .Let me show you a script i made.(Below)
(I do not expect you to read it)

// The delay, in milliseconds
// 1000 milliseconds = 1 second
var theDelay = 1000 * 1;

function canPwn() {
	if(window.lastPwnTime == null) return true;
	var now = new Date();
	var timeDifference = now - window.lastPwnTime;

	// Ensure we have waited enough time
	return timeDifference > theDelay;
}

function didPwn() {
	// We did a pwn, update the internal time
	window.lastPwnTime = new Date();
}

function checkPwn(force) {
	// Prevent pwning too fast
	if(!canPwn()) return;

    // ensure we have a store for seen values
    window.seenWords = window.seenWords || {};

    // This list seems dead
    //{"10":"handle","11":"part","38":"ping","58":"intel","59":"port","../client/img/word/m/17":"gridwidth","m/20":"accountname","e/60":"emit","m/9":"decryptfile","e/14":"signal","e/61":"loop","m/14":"datatype","e/41":"add","m/36":"getkey","e/15":"point","m/8":"download","e/33":"stat","e/20":"dir","e/59":"port","e/25":"host","e/13":"val","e/46":"type","e/4":"system","e/6":"left","../client/img/words/template.png":"a","h/24":"getxmlprotocol","m/52":"process","m/45":"package","h/39":"setnewproxy","m/13":"newline","h/50":"getpartoffile","m/60":"account","h/49":"getfirewallchannel","m/1":"gridheight","m/47":"server","e/16":"reset","e/35":"ghost","e/22":"write","e/3":"pass","e/12":"delete","e/29":"client","e/58":"intel","e/57":"set","e/37":"remove","e/50":"key","e/38":"ping","e/9":"anon","e/32":"http","h/18":"changeusername","h/45":"emitconfiglist","h/46":"fileexpresslog","h/4":"createfilethread","h/6":"removeoldcookie","h/37":"removenewcookie","h/7":"includedirectory","h/43":"blockthreat","h/15":"batchallfiles","h/16":"systemgridtype","h/10":"respondertimeout","h/3":"changepassword","h/51":"mergesocket","h/5":"disconnectserver","h/23":"sendintelpass","h/33":"channelsetpackage","h/38":"statusofprocess","h/21":"encodenewfolder","h/17":"getmysqldomain","h/28":"unpacktmpfile","h/8":"getdatapassword","h/42":"create2axisvector","h/11":"ghostfilesystem","h/36":"hostnewserver","h/47":"uploaduserstats","e/24":"get","e/21":"poly","e/17":"call","e/51":"right","e/26":"root","e/45":"domain","e/52":"user","e/49":"buffer","h/54":"disconnectchannel","h/2":"exportconfigpackage","h/30":"httpbuffersize","h/48":"bufferpingset","h/31":"patcheventlog","h/26":"rootcookieset","h/20":"create3axisvector","h/53":"checkhttptype","e/1":"join","e/31":"file","e/34":"list","e/43":"com","e/27":"url","e/19":"count","e/40":"upload","e/53":"info","e/23":"num","e/30":"log","e/54":"net","e/2":"data","e/48":"load","e/0":"cookies","e/8":"global","e/55":"init","e/39":"size","h/44":"dodecahedron","h/22":"eventlistdir","h/34":"destroybatch","h/29":"generatecodepack","h/27":"createnewpackage","h/32":"encryptunpackedbatch","h/12":"systemportkey","h/14":"sizeofhexagon","e/42":"bytes","e/28":"send","m/50":"writefile","e/11":"part","m/51":"listconfig","m/63":"decrypt","m/22":"password","m/33":"username","m/0":"command","h/41":"callmodule","h/0":"loadaltevent","h/35":"deleteallids","h/1":"wordcounter","h/40":"loadloggedpassword","h/25":"joinnetworkclient","h/9":"loadregisterlist","h/13":"createnewsocket"};

    // grab the string number of this
    var stringNumber = $('.tool-type-img').attr('src').replace('../client/img/word/','');

    // Prevent incorrect spamming
    var lastWordChoice = window.lastWordChoice;
    if(lastWordChoice == stringNumber && !force) return;
    window.lastWordChoice = stringNumber;

    // see if we've seen this before
    var possibleWord = window.seenWords[stringNumber];

    // Did we find the word?
    if(possibleWord != null) {
    	console.log('Found word ' + possibleWord);

        window.shouldntSubmit = true;
        $('#tool-type-word').val(possibleWord);

        $('#tool-type-form').submit();

        // We did a pwn
        didPwn()

        setTimeout(function() {
            $('#tool-type-word').val('');
            window.shouldntSubmit = false;
        }, 1);
    }
}

$('#tool-type-word').on('keyup keypress keydown', function(e) {
    if(window.shouldntSubmit) {
        e.preventDefault();
        return false;
    }

    if(e.which != 13) {
        return;
    }

    // Grab what we typed
    var currentEntry = $('#tool-type-word').val();

    // Did we type anything?
    if(currentEntry.length <= 0) {
        // Check if we should pwn
        checkPwn(true);

        e.preventDefault();
        return false;
    }

    // grab the string number of this
    var stringNumber = $('.tool-type-img').attr('src').replace('../client/img/word/','');

    // Store the answer
    window.seenWords[stringNumber] = currentEntry;

    // We did a pwn
    didPwn();
});

function exportBotMemory() {
	var theMemory = JSON.stringify(window.seenWords);

	console.log('To load bot memory, paste the following into the console:');
	console.log('window.seenWords = ' + theMemory + ';');
}

// Set an interval to check if we can pwn
setInterval(checkPwn, 100);

// Tell the user how to use it
console.log('Start typing words, eventually the bot will take over. You can type "exportBotMemory()" into the console without quotes to save your bot\'s current state.');
(function() {
    // Check if we've already started the hook
    if(window.ash47_pwnHook == null) {
        // Reset the images we've seen
        window.ash47_seenImages = window.ash47_seenImages || {};

        // Hook the image loading
        $('.tool-type-img').get()[0].onload = function() {
            window.ash47_pwnHook();
        }

        // Hook the form submission
        $('#tool-type-form').submit(function() {
            // We did a submission
            window.ash47_didSubmit();

            // Attempt to store the word
            window.ash47_storeWord();
        });

        // Anything we type automatically takes priority
        $('#tool-type-word').keyup(function() {
            window.ash47_storeWord();
        });

        // Add librarys
        window.addLibrary = function(loc) {
            var newScript = document.createElement('script');
            newScript.setAttribute('src', loc);
            document.head.appendChild(newScript);
        };

        // Library for image recognition
        window.addLibrary('https://cdn.rawgit.com/naptha/tesseract.js/1.0.10/dist/tesseract.js');
    }

    // The fastest we can push answers
    window.ash47_maxSubmitTime = 1000 * 1;
    window.ash47_bonusDelay = 100;

    // Export brain
    window.exportBrain = function() {
        console.log('window.importBrain(\'' + JSON.stringify(window.ash47_seenImages) + '\');');
    };

    // Import brain
    window.importBrain = function(newBrain, force) {
        try {
            var newSeenImages = JSON.parse(newBrain);

            for(var key in newSeenImages) {
                if(force || !window.ash47_seenImages[key]) {
                    window.ash47_seenImages[key] = newSeenImages[key];
                }
            }

            console.log('Brain successfullyy imported!');
        } catch(e) {
            console.log('Error importing brain!');
        }
    };

    // Add a line of info
    window.ash47_addline = function(txt) {
        var con = $('#cdm-text-container');
        
        con.append($('<div>', {
            text: txt
        }));

        con.scrollTop(con.prop('scrollHeight'));
    };

    // Attempt to store the word
    window.ash47_storeWord = function() {
        var currentValue = $('#tool-type-word').val();
        var stringNumber = $('.tool-type-img').attr('src');

        if(currentValue.length > 0) {
            window.ash47_seenImages[stringNumber] = currentValue;
        }
    };

    // Checks if we are allowed to submit
    window.ash47_canSubmit = function() {
        if(window.ash47_lastSubmission == null) return true;
        var now = new Date();
        var timeDifference = now - window.ash47_lastSubmission;

        // Ensure we have waited enough time
        return timeDifference > window.ash47_maxSubmitTime;
    };

    // Returns how long left until we can do the next submit
    window.ash47_howLongLeft = function() {
        if(window.ash47_lastSubmission == null) return 0;
        var now = new Date();
        var timeDifference = now - window.ash47_lastSubmission;

        if(timeDifference > window.ash47_maxSubmitTime) {
            return window.ash47_bonusDelay;
        } else {
            return timeDifference + window.ash47_bonusDelay;
        }
    };

    // Do a submission, IF WE ARE ALLOWED
    window.ash47_doSubmit = function() {
        // Are we allowed to submit?
        if(window.ash47_canSubmit()) {
            // Do the submit
            $('#tool-type-form').submit();

            // Ensure we actually mark us as recently submitted
            window.ash47_didSubmit();
        } else {
            // Sleep until we are allowed to submit
            setTimeout(function() {
                // Do the submit
                window.ash47_doSubmit();
            }, window.ash47_howLongLeft());
        }
    };

    // Marks that we just did a submission
    window.ash47_didSubmit = function() {
        window.ash47_lastSubmission = new Date();
    };

    // Define our pwnage hook
    window.ash47_pwnHook = function() {
        var stringNumber = $('.tool-type-img').attr('src');
        if(window.ash47_seenImages[stringNumber]) {
            // Store the value
            $('#tool-type-word').val(window.ash47_seenImages[stringNumber]);

            // Wait for the delay
            window.ash47_doSubmit();
            return;
        }

        // Yep, we got one! Grab it
        var ourImage = $('.tool-type-img').get()[0];

        // Try to read it
        Tesseract.recognize($('.tool-type-img').get()[0], {
            // Only allow lowercase and underscore
            tessedit_char_whitelist: 'abcdefghijklmnopqrstuvwxyz_23'
        })
            .then(function(result) {
                // Grab the text
                var res = result.text.trim().replace(/ /g, '');

                // Did we find something?
                if(res.length > 0) {
                    // Log what we see
                    window.ash47_addline('I see: ' + res);

                    // Store the value
                    $('#tool-type-word').val(res);

                    // Store the word
                    window.ash47_storeWord();

                    // Try to submit it
                    window.ash47_doSubmit();
                }
            });
    };
})();
