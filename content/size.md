---
Title: Size Tool
---

Copy your code below, and click submit.

<script>
var keywords = [
    'break',
    'case',
    'catch',
    'class',
    'const',
    'continue',
    'debugger',
    'default',
    'delete',
    'do',
    'else',
    'export',
    'extends',
    'finally',
    'for',
    'function',
    'if',
    'import',
    'in',
    'instanceof',
    'new',
    'return',
    'super',
    'switch',
    'this',
    'throw',
    'try',
    'typeof',
    'var',
    'void',
    'while',
    'with',
    'yield',
    'await',
    'null',
    'true',
    'false',
];

function isSpace(c) {
    return c === ' ' || c === '\t' || c === '\n' || c === '\r' || c === '\f';
}

function isLowercase(c) {
    c = c.charCodeAt(0);
    return c > 96 && c < 123;
}

function isCtrl(c) {
    return c === ';' || c === '{' || c === '}';
}

function checkCode() {
    var elsize = document.getElementById('size-span');
    var elbytes = document.getElementById('bytes-span');

    var code = document.getElementById('code-textarea').value;

    /* Full size: */
    elsize.innerHTML = code.length + ' bytes';
    if (code.length > 4096) {
        elsize.style.color = 'red';
    } else {
        elsize.style.color = 'green';
    }

    /* Counted bytes: */
    var bytes = 0;
    var word = '';
    var prev = '';
    var c = code[0];
    if (!isSpace(code[code.length - 1]))
      code += '\n';
    for (var i = 0; i < code.length; ++i, prev = c, c = code[i]) {

        /* Currently in a word? */
        if (isLowercase(c)) {
            word += c;
            continue;
        }

        /* End of word? */
        if (word.length > 0) {
            /* Word not a keyword. Count it in full! */
            if (!keywords.find(function(e){ return e === word; })) {
                bytes += word.length;
                word = '';
                continue;
            } else {
                bytes += 1;
                word = '';
            }
        }

        /* Ignore any spaces. */
        if (isSpace(c)) {
            /* Ignore curly braces and semicolons if followed by whitespace. */
            if (isCtrl(prev)) {
                bytes -= 1;
                continue;
            }

            continue;
        }

        /* Count the byte as normal. */
        bytes++;
    }

    elbytes.innerHTML = bytes.toString();
    if (bytes > 2048) {
        elbytes.style.color = 'red';
    } else {
        elbytes.style.color = 'green';
    }
}

</script>

<form onsubmit="return false">
    <textarea id="code-textarea" name="code" cols="80" rows="20"></textarea>
    <br>
    <input type="submit" value="Update" onclick="checkCode()">
</form>

**Full size:** <span id="size-span"></span><br>
**Counted bytes:** <span id="bytes-span"></span>

The full size must not exceed 4096 bytes (rule §2.1).<br>
The number of counted bytes must not exceed 2048 (rule §2.2).
