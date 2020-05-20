<script>

    let strength = 0;
    let validations = [];
    let showPassword = false;

    function validatePassword(e) {
        const password = e.target.value;

        validations = [
            (password.length >= 5),
            (password.search(/[A-Z]/) > -1),
            (password.search(/[0-9]/) > -1),
            (password.search(/[$&+,:;=?@#]/) > -1),
        ]

        strength = validations.reduce((acc, cur) => acc + cur)
    }
</script>

<style>
    form{
        --text-color: #afafaf;
        text-align: center;
        padding: 145px 555px;
    }

    .field {
        width: 100%;
        position: relative;
        border-bottom: 2px dashed var(--text-color);
        margin: 2rem auto 1rem;
    }

    .label {
        color: var(--text-color);
        font-size: 1.2rem;
    }

    .input {
        text-align: center;
        outline: none;
        border: none;
        overflow: hidden;
        margin: 0;
        width: 100%;
        padding: 0.25rem 0;
        background: none;
        color: white;
        font-size: 1.2rem;
        font-weight: bold
    }

    .input:valid {
        color: yellowgreen;
    }

    .input:invalid {
        color: orangered;
    }

    /* Animação da borda */

    .field::after {
        content: "";
        position: relative;
        display: block;
        height: 4px;
        width: 100%;
        background:#ffd700;
        transform: scaleX(0);
        transition: transform 500ms ease;
        top: 2px;
    }

    .field:focus-within {
        border-color: transparent;
    }

    .field:focus-within::after {
        transform: scaleX(1);
    }

    /* Animação do Label */

    .label {
        z-index: -1;
        width: 100%;
        text-align: center;
        position: absolute;
        transform: translateY(-2rem);
        transition: transform 400ms;
    }

    .field:focus-within .label,
    .input:not(:placeholder-shown) + .label {
        transform: scale(0.8) translateY(-5rem);
    }

    /* Contador de força da senha */

    .strength {
        display: flex;
        height: 20px;
        width: 100%;
    }

    .bar {
        margin-right: 5px;
        height: 100%;
        width: 25%;
        transition: box-shadow 500ms;
        box-shadow: inset 0px 20px #1f1f1f;
    }

    .bar-show {
        box-shadow: none;
    }

    .bar-1 {
        background: linear-gradient(to right, red, orangered);
    }

    .bar-2 {
        background: linear-gradient(to right, orangered, yellow);
    }

    .bar-3 {
        background: linear-gradient(to right, yellow, yellowgreen);
    }

    .bar-4 {
        background: linear-gradient(to right, yellowgreen, green);
    }

    .toggle-password {
        position: absolute;
        cursor: help;
        font-size: 0.8rem;
        right: 0.25rem;
        bottom: 0.5rem;
    }

    .bot {
        position: relative;
        width: 100%;
        background-color: transparent;
        border-radius: 29px;
        color: var(--text-color);
    }

    @media (max-width: 1592px) {
        form {
            padding: unset;
            padding-top: 32px;
        }
    }

</style>

<main>
    <form>

        <div class="field">
            <input type="email" name="email" class="input" placeholder=" " />
            <label for="email" class="label">E-mail</label>
        </div>

        <div class="field">
            <input type={showPassword ? 'text' : 'password'} class="input" placeholder=" " on:input={validatePassword} />
            <label for="password" class="label">Senha</label>

            <span 
                class="toggle-password"
                on:mouseenter={() => (showPassword = true)}
                on:mouseleave={() => (showPassword = false)}>
                {showPassword ? '🙈' : '👁️'}
            </span>
        </div>

        <div class="strength">
            <span class="bar bar-1" class:bar-show={strength > 0} />
            <span class="bar bar-2" class:bar-show={strength > 1}/>
            <span class="bar bar-3" class:bar-show={strength > 2}/>
            <span class="bar bar-4" class:bar-show={strength > 3}/>
        </div>

        <ul style="text-align: left">
            <li> {validations[0] ? '✔️' : '❌'} deve ter ao menos 5 caracteres</li>
            <li> {validations[1] ? '✔️' : '❌'} deve conter uma letra maiúscula</li>
            <li> {validations[2] ? '✔️' : '❌'} deve conter um número</li>
            <li> {validations[3] ? '✔️' : '❌'} deve conter um dos seguintes símbolos: $&+,:;=?@#</li>
        </ul>

        <button class="bot">Login</button>

    </form>
</main>