The frontend takes 'palette-template.ino' and performs a
substitution on it.  For example:

    var program = template.replace(/PIN0_EFFECT/g, 'FADE')
                          .replace(/PIN0_SPEED/g, '5')
                          .replace(/PIN0_RANDOMNESS/g, '0')
                          .replace(/PIN0_BRIGHTNESS/g, '100')
                          .replace(/PIN1_EFFECT/g, 'TWINKLE')
                          .replace(/PIN1_SPEED/g, '5')
                          .replace(/PIN1_RANDOMNESS/g, '0')
                          .replace(/PIN1_BRIGHTNESS/g, '100')
                        ... continue through to PIN5

    var config = {
        'compileUrl': '//ltc.chibitronics.com/compile'
    };

    var codeObject = {
        'files': [{
            'filename': 'project.ino',
            'content': processedProgram,
        }],
        'format': 'binary',
        'version': '167',
        'libraries': [],
        'fqbn': 'chibitronics:esplanade:code'
    };

    var request = new window.XMLHttpRequest();
    request.onreadystatechange = function () {
        if (request.readyState === 4) {
            modulateAndPlay(request.responseText);
        }
    };
    request.open('POST', config.compileUrl, true);
    request.send(JSON.stringify(codeObject));

This then runs it through a modulation.  This can be handled by https://www.npmjs.com/package/chibitronics-ltc-modulate
