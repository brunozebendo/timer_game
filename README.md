# timer_game
Jogo para acertar o tempo, aprendendo conceitos de fragments, refs, portals, entre outros

/**O objetivo do código é criar um programa com quatro contadores de tempo
 * onde o usuário tem que clicar em um botão para iniciar e parar, tentando 
 * acertar o tempo dado. A ideia é introduzir os módulos de portals e useref
 */
/**O useRef vai substituit o useState em alguns casos que não se precisa
 * seguir o estado alterado do elemento, no caso desse programa, foi criada
 * a variável abaixo
 */
const playerName = useRef();
/**que dentro do return é passada como abaixo, e, assim, o componente input
 * está conectado com a variável acima
 */
<input
    ref={playerName}
    type="text"
/>
/**Assim, pode-se usar essa conecção para explorar a propriedade current.value,
 * ou seja, dentro da função set... que é o nome alterado, passa-se o valor corrente
 * do ref
 */
function handleClick() {
    setEnteredPlayerName(playerName.current.value);
  }
/**também é usada a sintaxe abaixo no lugar do if, else */
  
/**também é usada a sintaxe abaixo no lugar do if, else */

<h2>Welcome {enteredPlayerName ?? 'unknown entity'}</h2>

/**não esquecer de passar a função no button */

    <button onClick={handleClick}>Set Name</button></>

/**Vale destacar que apesar de usar o useRef também usou o useState pois como
 * o nome inserido no input precisava ser atualizado na mensagem de Welcome o
 * useState é que faz esse papel de re-renderização, por isso o State deve ser 
 * usado para elementos que se refletem no UI e Refs para valor "behind the scenes"
 * e para acessar diretamente certos elementos do DOM, como o valor, no presente caso
 */
/**Na aula 145 é criado o componente para os componentes onde ficará o layout básico do
 * TimerChallenge. Abaixo, copio o componente, destacando que conforme ia criando o componente, o 
 * professor já incluia o nome no props, a lógica é já ir pensando o que será passado para o app e lá
 * assumirá diferentes valores, no caso do código, serão quatro quadros com diferentes valores. O código
 * abaixo não está completo
 */

export default function TimerChallenge({title, targetTime}){
    return <section className="challenge">
        <h2>{title}</h2>
        <p className="challenge-time">
        {targetTime} second {targetTime>1 ? 's' : ''}
        </p>
        <p>
            Start Challenge
        </p>
        <p className="">
            time is running.../timer inactive
        </p>
    </section>
}

/**Então, no return do APP foi passado o seguinte código, ou seja, exibindo o componente, mas
 * adaptando o título e o tempo.
*/

<TimerChallenge title="Easy" targetTime={1} />
<TimerChallenge title="Not Easy" targetTime={5} />
<TimerChallenge title="Getting Tough" targetTime={10} />
<TimerChallenge title="Pros only" targetTime={15} />

/**Na aula 146 vai ser inserida a lógica para o o botão de iniciar e parar a contagem, dentro 
 * do componenete TimerChallenge. Então a função abaixo vai cuidar da verificação se o tempo expirou
 * e assim o jogador perdeu. A lógica é que o setTimerExpired for atingido, ou seja, for true, o jogador
 * perde. Pelo que entendi,se o targetTime que aqui está multiplicado por milisegundos, for atingido, o
 * setTimerExpired é true e o parágrafo dentro do return, mostra condicionalmente a mensagem mais abaixo.
 *Lembrando que eu sempre leio o && como então. Também foram criadas useStates para cada um desse set...
 */

function handleStart () {
        
    setTimeout(() => {
        setTimerExpired(true)
    }, targetTime * 1000);
    setTimerStarted(true)
}

{timerExpired && <p>You lost!</p>}

/**a função foi conectada ao botão */
<button onClick={handleStart}></button>
/**Também foi criada um useState para controlar quando o tempo foi iniciado, passado na mesma função
 * acima e servindo de controle de componente, conforme abaixo, onde o texto e mesmo a classe são mostrados
 * de acordo com o estado do timeStarted, ou seja, se o botão foi clicado e o tempo iniciado ou não
 */

{timerStarted ? 'Stop' : 'Start'}
<p className={timerStarted ? 'active' : undefined}>
{timerStarted ? 'Time is running...':'Timer inactive'}
</p>
/*Já na aula 147 vai ser criado um ref para controlar o tempo de cada componente, como é um
controle interno que não afeta a UI, o ideal é usar useRef. Então foi criada uma variável
que é passada como useRef,*/

const timer=useRef();

/**Então a a variável timer utiliza a função current para pegar o valor corrente do setTimeout que,
 * lembrando, é a função que aciona o tempo e esse valor é passado para a função abaixo que serve para 
 * limpar o tempo do componente quando o botão é clicado
 */

function handleStart (){
    timer.current = setTimeout(() => {
            setTimerExpired(true)
        }, targetTime * 1000);
        setTimerStarted(true)
    }
    function handleStop(){
        clearTimeout(timer.current);
    }

    /**Então o botão é atualizado para receber condicionalmente as funções */

    <button onClick={timerStarted ? handleStop : handleStart}></button>
/**Na aula 148 vai ser criado um pop up para mostrar o resultado e os 
 * pontos que atingiu, para isso, foi criado um novo componente, ResultModal.jsx
 * Abaixo o componente que usa a sintaxe padrão. Vale destacar aqui a tag
 * dialog que é um padrão para mostrar essas caixas de diálogo (tipo um alert)
 * Reparar também na tag strong para destacar os itens e que dentro do form
 * foi chamado o method="dialog" com um botão de Close, o que é uma sintaxe 
 * básica para fechar a caixa de diálogo automaticamente.
 */

export default function ResultModal({result, targetTime}){
    return( <dialog className="result-modal">
        <h2>You {result}</h2>
        <p>The target ime was <strong> {targetTime} seconds.</strong></p>
        <p>You stopped the timer with <strong></strong></p>
        <form method="dialog">
            <button>Close</button>
        </form>
    </dialog>);
}

/**Então, dentro do elemento TimerChallenge, é inserida a linha abaixo que vai
 * mostrar condicionalmente o caixa do dialog se o jogador perder.
 */

{timerExpired && <ResultModal targetTime={targetTime} result="lost" />}

/**A aula 149 vai usar ref para passar a caixa de diálogo para o componente
 * TimerChallenge. Primeiro é criado o ref no TimerChallenge
 */
const dialog = useRef();
/**E esse ref é passado dentro do component que fica dentro do return */

<ResultModal ref={dialog} targetTime={targetTime} result="lost" />
/**Então a função abaixo é atualizada para receber a linha dialog.current.showModal();
 * Sendo dialog o nome do ref, current o valor corrente do tempo (acho) e showModal()
 * uma propriedade do JS para mostrar o dialog
 */

function handleStart (){
    timer.current = setTimeout(() => {
            setTimerExpired(true);
            dialog.current.showModal();
        }, targetTime * 1000);
        setTimerStarted(true);
    }

/**No entanto, para passar um componente para outro componente, é preciso
 * importar uma função especial, forwardRef, que faz exatamente isso
 * permite que o ref de um componente seja usado em outro. Atentar para
 * a restruturação necessária no componente para que o componente
 * esteja envelopado dentro da função e para que o ref seja passado como
 * um segundo componente, depois do props, além de dentro do return.
 */

import { forwardRef } from "react";

const ResultModal = forwardRef ( function ResultModal({ result, targetTime}, ref){

    return( <dialog ref={ref} className="result-modal">
        <h2>You {result}</h2>
        <p>The target ime was <strong> {targetTime} seconds.</strong></p>
        <p>You stopped the timer with <strong></strong></p>
        <form method="dialog">
            <button>Close</button>
        </form>
    </dialog>);
});

export default ResultModal;

/**A aula 150 vai explicar o useImperativeHandle que vai mudar a forma como
 * o ref é passado para o outro componente, o useImperativeHandle normalmente
 * trabalha com o forwardRef. Assim, ele é importado no começo do código, e depois
 * é usada essa sintaxe abaixo, onde, dialog passa a ser o ref e não mais a tag
 * dialog já que a tag pode ser mudada e se perderia a passagem do ref
  */
const dialog = useRef();

useImperativeHandle(ref, () =>{
    return {
        open() {
            dialog.current.showModal()
        }
    }
});

return( <dialog ref={dialog} className="result-modal">
</dialog>

/**Já no componente TimerChallenge, dentro da função HanlderStart é mudada
 * a função para chamar o que foi chamado de open
 */

dialog.current.open();

/**A aula 151 vai mudar o código para que seja mostrado o tempo restante no módulo e seja dada uma 
 * pontuação de acordo com o tempo faltante para acertar o tempo. No entanto o setTimeOut não dá essa
 * opção, assim, vai ser usar o setInterval. Também será necessário mudar a lógica dos states, onde, 
 * antes tinham dois states, um para controlar quando o componente foi iniciado e outro, quando foi
 * parado, agora vai ser só um para o tempo restante. Sendo o estado inicial o tempo alvo vezes 1000
 * milisegundos.
 */

const [timeRemaining, setTimeRemaining] = useState(targetTime * 1000);

/**Assim, a função fica com a seguinte lógica, o 10 no final significa que a função deve ser executada
 * a cada dez milisegundos, então o tempo corrente vai ser o tempo anterior menos 10 milisegundos, 
 */

function handleStart (){
    timer.current = setInterval(() => {
       setTimeRemaining(prevTimeRemaining => prevTimeRemaining - 10)     
        }, 10);

/**Também é inserida a seguinte lógica para controlar se o tempo foi iniciado, ou seja, se ele é maior
 * que zero e menor que o tempo alvo.
*/

const timerIsActive = timeRemaining > 0 && timeRemaining < targetTime * 1000;

/**Aqui foi inserida a lóica para limpar o contador quando o tempo for atingido sem que o jogador 
 * clique no stop, se o tempo restante for menor ou igual a zero, a função clearInterval zera o
 * tempo corrente e a função setTimeRemaining volta ele para o tempo inicial, também é chamado
 * novamente o useref criado no outro componente para poder mostrar o modal
 */
if (timeRemaining <= 0 ) {
    clearInterval(timer.current);
    setTimeRemaining(targetTime * 1000);
    dialog.current.open();
}
/**A lógica da função abaixo é muito parecida com a de cima, mas aqui ela vai ser passada
 * para o botão, para assim o componente ser parado manualmente.
 */
function handleStop() {
    dialog.current.open();
    clearInterval(timer.current);
    }
/**Dentro do return o código é atualizado para o novo nome do controlador timerIsActive  */

    
        /**Dentro do return o código é atualizado para o novo nome do controlador timerIsActive  */
        <>
        /**Dentro do return o código é atualizado para o novo nome do controlador timerIsActive  */
        /**Dentro do return o código é atualizado para o novo nome do controlador timerIsActive  */
        <button onClick={timerIsActive ? handleStop : handleStart}>
            {timerIsActive ? 'Stop' : 'Start'} Challenge
        </button>
        /**Na aula 152,  a informação do tempo restante tem que ser passada do componente TimerChallenge
        * para o ResultModal e lá possa ser calculado um score, para isso é criado o prop abaixo (remainingTime).
        Reparar novamente na lógica do prop, ele é passado dentro do return do componente que detem
        a informação que se quer passar e é usado no componente receptor
        */
               <ResultModal
                ref={dialog}
                targetTime={targetTime}
                remainingTime={timeRemaining}
                onReset={handleReset} /></>

/**Assim, ele é recebido no ResultModal com um prop */

const ResultModal = forwardRef ( function ResultModal({ targetTime, remainingTime, onReset

/**é criada a variável abaixo para controlar quando o usuário perdeu, reparar como aqui
 * o prop já está sendo usado */    

const userLost = remainingTime <= 0;

/**e no return ele é usado condicionalmente */

{userLost && <h2>You lost</h2>}

/**Então é criada a seguinte fórmula para formatar o tempo restante em duas casas decimais e este
 * é retornado no return
 */

const formattedRemainingTime = (remainingTime / 1000).toFixed(2);

<p>You stopped the timer with <strong>{formattedRemainingTime} seconds left.</strong></p>

/**então foi criada uma função separada no TimerChallenge para resetar o tempo */

function handleReset () {
    setTimeRemaining(targetTime * 1000);
}
/**ela também é passada como props */

return(
    <>
    <ResultModal
    ref={dialog}
    targetTime={targetTime}
    remainingTime={timeRemaining}
    onReset={handleReset}
    />

/*e recebida no ResultModal*/</>

const ResultModal = forwardRef ( function ResultModal({ targetTime, remainingTime, onReset}

/**e usada na função onSubmit para resetar o contador, ou seja, quando a tag form for submetida
 * Então, repetindo, é criada a função, essa função é um atributo para o props q é passado
 * dentro do elemento no return, passado novamente no elemento que vai recebe-lo e novamente
 * passado dentro do método que vai ser usado e, assim, o método, por fim, vai chamar a função.
 */

<form method="dialog" onSubmit={onReset}>
            <button>Close</button>
        </form>

/**Na aula 153 vai ser incluído o cálculo do score, a pontuação de acordo com o tempo restante
 * quando o tempo for pausado. Para isso foi criada a variável abaixo que guarda o resultado
 * da diferença entre o tempo restante e o tempo alvo, menos um multiplicado por cem, assim
 * o score ficará entre 0 e 100, o 1000 foi usado para igualar as bases já que o targetime 
 * estava em segundos.
 */

const score = Math.round((1 - remainingTime /( targetTime * 1000)) * 100);

/**Assim, no retorno, é passada a lógica abaixo para mostrar o score somente se
 * o jogador não tiver perdido
 */
{!userLost && <h2>Your Score:{score}</h2>}

/**A tag dialog por padrão fecha quando o ESC é pressionado, no entanto, isso não acionaria
 * a função onReset, ou seja, não voltaria o componente para o padrão inicial. Para isso
 * foi incluído o prop built-in onClose.
 */

return( <dialog ref={dialog} className="result-modal" onClose={onReset}></dialog>
/**A aula 155 fala de portals, que já expliquei antes, mas, em resumo, é uma forma de fazer
 * com que elementos aninhados na árvore DOM sejam renderizados em outro local da estrutura
 * mais perto do body, por exemplo. No caso desse código, isso será feito com o JSX do resultModal
 * que é o componente que mostra o resultado, ou seja, ele é muito usado para alerts e modals.
 * Primeiro, é preciso fazer a importação abaixo. Ressaltando que quando se importa do react, 
 * são features que funcionam em todos os ambientes, como react native, por exemplo, e 
 * a react-dom é para Browsers.
 */

import {createPortal} from "react-dom";

/**Segundo, antes do elemento a ser exportado coloca-se o comando abaixo e no final dele o segundo
 * argumento que vai levar o nome de um elemento que vai ser incluído e o nome do atributo
 * vai ser uma tag que deve ser incluída no index.html
 */

return createPortal (
    ...
    document.getElementById('modal') 
/**a tag abaixo está no index.html perto do body, para não ficar tão aninhado */
    <body>
    <div id="modal"></div>
