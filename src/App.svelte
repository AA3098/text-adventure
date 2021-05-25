<script>
  import { onDestroy } from 'svelte';
  import { fly, blur, scale } from 'svelte/transition';
  import { elasticOut } from 'svelte/easing';

  import Header from './Header.svelte';
  import Login from './Login.svelte';
  import Backpack from './Backpack.svelte';
  import Nappi from './Nappi.svelte';
  import Kuolema from './Kuolema.svelte';
  import Keittio from './Keittio.svelte';
  import KillerAjastin from './KillerAjastin.svelte';
  import Piilokaikka from './Piilopaikka.svelte';
  import inventory from './inventory';
  import Piilopaikka from './Piilopaikka.svelte';

  // alkutietoja
  let name = '';
  let slogan = 'Enter the dungeon, if you dare!';

  //inventory sub/unsub

  let items;

  const unsub = inventory.subscribe((x) => {
    items = x;
  });

  onDestroy(() => {
    if (unsub) {
      unsub();
    }
  });

  //progress
  let step = 0;

  const login = (ce) => {
    step++;
    //lisätään reppuun itemi
    inventory.update((aiemmat) => [...aiemmat, 'torch']);
    //slogan päivitys
    slogan = 'You will never escape alive!';

    //nimen päivitys
    name = ce.detail;
    name === name;
  };
  //askeleet

  const stepForward = () => {
    step++;
  };

  const halfForward = () => {
    step = step + 0.5;
  };

  const stepBackward = () => {
    step--;
  };

  const halfBackward = () => {
    step = step - 0.5;
  };

  const superHalfStep = () => {
    step = step + 1.5;
  };

  const superFullStep = () => {
    step = step + 2;
  };

  //puzzle 1

  //teksti toggle
  let vartija1 = 0;

  const vartijatoggle = () => {
    vartija1 = 1;
    vartija1 = vartija1;
    stepBackward();
  };
  //omenan poistaminen repusta
  const puzzle1valmis = () => {
    inventory.set(['torch']);
    halfForward();
  };

  //puzzle 2

  //teksti toggleja
  let onkoKaytyp2 = 0;

  const halfonKaytyp2 = () => {
    onkoKaytyp2 = 1;
    halfForward();
  };

  const fullonKaytyp2 = () => {
    onkoKaytyp2 = 1;
    stepForward();
  };
  //item modaalin toggle
  let hella = false;

  const hellatoggle = () => {
    hella = !hella;
  };
  //onko aaveilla ruokaa
  let ruokaesilla = false;
  //tarjoa aaveille ruokaa
  const presentFood = () => {
    if (items[1] === 'pot' || items[1] === 'kettle') {
      p2kuolema1();
    } else if (items[1] === 'cauldron') {
      ruokaesilla = !ruokaesilla;
      inventory.set(['torch']);
    }
  };
  //käyttikö pelaaja oikean itemin
  const dininghall = () => {
    if (ruokaesilla) {
      halfForward();
    } else {
      p2kuolema2();
    }
  };

  // puzzle 2 itemin valinta

  const puzzle2pot = () => {
    inventory.set(['torch', 'pot']);
  };

  const puzzle2kettle = () => {
    inventory.set(['torch', 'kettle']);
  };

  const puzzle2cauldron = () => {
    inventory.set(['torch', 'cauldron']);
  };

  //puzzle 3

  //pimeyden hallitseminen
  let pimeys = true;

  const pimeystoggle = () => {
    pimeys = !pimeys;
    inventory.set([]);
  };

  //huoneisiin astuminen

  //mahdollisuus tappajan ilmestymiseen
  let onkoVaarassa = false;

  const vaaratoggle = () => {
    onkoVaarassa = !onkoVaarassa;
    console.log('vaara toggle');
  };

  //oletko piilossa
  let onkoPiilossa = false;

  const piilotoggle = () => {
    onkoPiilossa = !onkoPiilossa;
  };

  //varoitus ajastimen näkyvyys
  let killervaroitus = false;

  const varoitustoggle = () => {
    killervaroitus = !killervaroitus;
  };

  //puzzle 3 vaara huoneisiin meno
  const enterWest = () => {
    vaaratoggle();
    tappajaTikitys();
    halfForward();
  };

  const enterNorth = () => {
    vaaratoggle();
    tappajaTikitys();
    stepForward();
  };

  const enterEast = () => {
    vaaratoggle();
    tappajaTikitys();
    superHalfStep();
  };

  const goBackp3 = () => {
    vaaratoggle();
    step = 6;
  };

  //tappaja kuvien esiintyminen
  let kello = false;
  //tappajan kuvien esiintyminen
  const kellotoggle = () => {
    kello = !kello;
    varoitustoggle();
    setTimeout(() => {
      kello = !kello;
    }, 2000);
    //jollet ole piilossa, kuolet
    if (!onkoPiilossa) {
      setTimeout(() => {
        p3kuolema();
      }, 2100);
    }
  };

  //kun pelaaja on vaarallisessa huoneessa, "kello" tikittää koska murhaaja saapuu huoneeseen
  let tikitys;

  const tappajaTikitys = () => {
    if (!kello && onkoVaarassa) {
      //tehdään setIntervalille id
      let trigger = setInterval(() => {
        tikitys = Math.floor(Math.random() * 10);
        console.log('tikittää');
        if (tikitys <= 2) {
          varoitustoggle();
          //pysäytetään setInterval
          clearInterval(trigger);
        }
      }, 1000);
    }
  };

  //puzzle 3 huoneiden etsimiseen "väärä paikka" tekstin toggle
  let wrongplace = false;

  const wrongtoggle = () => {
    wrongplace = !wrongplace;
    setTimeout(() => {
      wrongplace = !wrongplace;
    }, 3000);
  };

  //reppu näkyvyys
  let reppu = false;

  const repputoggle = () => {
    reppu = !reppu;
  };

  //mitä tapahtuu kuoleman jälkeen
  let deaths = 0;
  let deathreason = 0;

  let deathwindow = false;

  //puzzle kuolemat
  const p1kuolema = () => {
    deathreason = 1;
    deathtoggle();
  };

  const p2kuolema1 = () => {
    deathreason = 2;
    deathtoggle();
  };

  const p2kuolema2 = () => {
    deathreason = 3;
    deathtoggle();
  };

  const p3kuolema = () => {
    deathreason = 4;
    deathtoggle();
  };
  //kuolemaikkunan näkyvyys
  const deathtoggle = () => {
    deathwindow = !deathwindow;
  };
  //pelin resetointi ensimmäiseen puzzlehuoneeseen
  const menitKuolemaan = () => {
    deaths++;
    deathreason = 0;
    step = 2;
    deathtoggle();
    inventory.set(['torch']);
  };

  const uudestaan = () => {
    step = 2;
    inventory.set(['torch']);
  };

  const lopetus = () => {
    step = 0;
  };
</script>

<main>
  <!--reppu näkyvyys-->
  {#if reppu}
    <Backpack />
  {/if}
  <!--kuolemaikkunan näkyvyys-->
  {#if deathwindow}
    <Kuolema {deathreason} on:click={menitKuolemaan} />
  {/if}
  <!--puzzle 2 keittiökalujen näkyvyys-->
  {#if hella}
    <Keittio
      on:takepot={puzzle2pot}
      on:takekettle={puzzle2kettle}
      on:takecauldron={puzzle2cauldron}
      on:takapakki={hellatoggle}
    />
  {/if}
  <!--tappaja varoitusikkunan näkyvyys-->
  {#if killervaroitus}
    <KillerAjastin on:hide={kellotoggle} {killervaroitus} />
  {/if}
  <!--tappajan kuvan näkyvyys-->
  {#if kello}
    {#if !onkoPiilossa}
      <div
        class="tappaja"
        in:fly={{ duration: 1000, x: 0, y: 500 }}
        out:scale={{ duration: 300 }}
      >
        <img
          src="./img/killer1.png"
          alt="killer full face"
          width="600"
          height="600"
        />
      </div>
    {:else if onkoPiilossa}
      <div
        class="tappajapeek"
        in:fly={{ duration: 1000, x: -500, y: 0 }}
        out:fly={{ duration: 300, x: -500, y: 0 }}
      >
        <img
          src="./img/killer2.png"
          alt="killer peeking"
          width="324"
          height="600"
        />
      </div>
    {/if}
  {/if}
  <!--Piilopaikan näkyvyys puzzle 3-->
  {#if onkoPiilossa}
    <Piilopaikka on:click={piilotoggle} />
  {/if}
  <div class="container">
    <header>
      <Header {slogan} />
    </header>
    {#if step === 0}
      <!--pelaajan nimen syöttö-->

      <Login on:nimeys={login} />
    {:else if step === 1}
      <!--tervetuloa viesti ja ensimmäinen itemi-->
      <div class="storybox" in:blur={{ duration: 1000 }}>
        <h1>Welcome {name}, you will need these on your journey.</h1>
        <h2>*Backback acquired*</h2>
        <h2>*Torch acquired*</h2>
      </div>
      <div class="päätösboxi" in:blur={{ duration: 1000 }}>
        <button on:click={stepForward}>Begin your journey</button>
      </div>

      <!--puzzle 1 alkaa-->
    {:else if step === 2}
      <!--pelaaja saapuu ensimmäiseen huoneeseen-->

      <div class="storybox" in:blur={{ duration: 1000 }}>
        <p>
          You realise you have been transported in front of a castle. The castle
          seems to be in ruins after what you imagine to be hundreds of years.
        </p>
        {#if deaths > 0}
          <p>
            The castle seems familiar to you, like you have been here before.
          </p>
        {/if}
        <p>
          Short walk away from the castle you notice a small apple tree. The
          tree seems almost withered away but it still bears few fruits on its
          branches.
        </p>
        <p>
          Once you notice the castle’s gate, you almost jump in fear. In front
          of the gate stands a large monster over half your own size, half man
          and half beast with large muscles, but strangely looking malnourished.
          The monster is holding on to a large halberd and is covered in matted
          fur and scars. It seems the large monstrosity has taken a notice of
          you as well, as it is looking straight at you with drool dripping from
          its jaws.
        </p>
        {#if vartija1 === 1}
          <p>
            Exhausted from running away from the monster, you notice it does not
            follow you.
          </p>
        {/if}
      </div>
      <br />
      <p>What will you do?</p>
      <div class="päätösboxi" in:blur={{ duration: 1000 }}>
        <Nappi on:click={stepForward}>Approach the beast</Nappi>
        <Nappi on:click={halfForward}>Investigate the tree</Nappi>
      </div>

      <!--seuraava vaihe-->
    {:else if step === 2.5}
      <!--pelaaja saapuu omenapuun luo-->

      <div class="storybox" in:blur={{ duration: 1000 }}>
        <p>You decide to walk to the apple tree.</p>
        <br />
        <p>
          You are now standing under the half dead fruit tree. To your amazement
          it still bears few apples on it's branches.
        </p>

        <!--kun puusta otetaan omena-->
        {#if items[1] === 'apple'}
          <h2>*Apple acquired*</h2>
        {/if}
      </div>
      <br />
      <p>What will you do?</p>
      <div class="päätösboxi" in:blur={{ duration: 1000 }}>
        <button
          disabled={$inventory[1] === 'apple'}
          on:click={() => inventory.update((aiemmat) => [...aiemmat, 'apple'])}
          >Take apple</button
        >
        <Nappi on:click={halfBackward}>Go back the way you came</Nappi>
      </div>

      <!--seuraava vaihe-->
    {:else if step === 3}
      <!--pelaaja saapuu vartijan luo-->
      <div class="storybox" in:blur={{ duration: 1000 }}>
        <p>
          You approach the towering monster. As you get near it, the moster
          starts to drool more and more. Once you have stopped just a few meters
          away from it, you can hear the drool from its jaws drip on the ground.
          As hungry as the monster guard seems, it does not move away from its
          post.
        </p>
      </div>
      <p>what will you do?</p>
      <div class="päätösboxi" in:blur={{ duration: 1000 }}>
        {#if items[1] === 'apple'}
          <Nappi on:click={puzzle1valmis}>Give apple</Nappi>
        {/if}
        <Nappi on:click={p1kuolema}>Try to pet the monster</Nappi>
        <Nappi on:click={vartijatoggle}>Run back</Nappi>
      </div>

      <!--puzzle 1 päätös-->
    {:else if step === 3.5}
      <!--pelaaja toteaa vartijalle, että "syöppä omena"-->

      <div class="storybox" in:blur={{ duration: 1000 }}>
        <p>
          The monster looks at you, seeming almost dumbfounded on what you are
          doing. After a few moments more the large beast takes the apple from
          your hand and opens the gate for you, out of kindness or orders, you
          do now know nor want to find out.
        </p>
        <h2>*You lost Apple*</h2>
        <p>Will you step inside the castle?</p>
      </div>
      <div class="päätösboxi" in:blur={{ duration: 1000 }}>
        <Nappi on:click={halfForward}>Step inside</Nappi>
      </div>

      <!--puzzle 2 alkaa-->
    {:else if step === 4}
      <!--pelaaja saapuu linnan eteiseen-->

      <div class="storybox" in:blur={{ duration: 1000 }}>
        {#if onkoKaytyp2 === 0}
          <p>
            As you step in the lobby of the castle, the chandelier above you and
            all of the candles in the room lit up. It seems someone was
            expecting visitors.
          </p>
        {/if}
        <p>
          As you walk inward deeper into the castle you eventually arrive at a
          fork in the road, the path seems to spilt in two.
        </p>
        <p>
          From the left path you can hear noises of an angry growd and
          flickering lights.
        </p>
        <p>
          From the right path you can hear the distict noises of different
          cookware clanking and the smell of food being prepared.
        </p>
      </div>
      <p>What will you do?</p>
      <div class="päätösboxi" in:blur={{ duration: 1000 }}>
        <Nappi on:click={fullonKaytyp2}>Take the left path</Nappi>
        <Nappi on:click={halfonKaytyp2}>Take the right path</Nappi>
      </div>

      <!--seuraava vaihe-->
    {:else if step === 4.5}
      <!--pelaaja saapuu keittiöön-->

      <div class="storybox" in:blur={{ duration: 1000 }}>
        <p>
          You arrive at what seems to be the castle's kitchen. Dust and
          spiderwebs cover the table tops and shelfs, no one has been here in
          years. As you step foot inside the kitchen proper, ghostly apparitions
          appear in front of you.
        </p>
        <p>
          The ghosts seem to be dressed as chef's and kitchen staff. They go on
          about their otherwordly duties and do not seem to notice you.
        </p>
        <p>
          Over at the stove there seems to be three sets of cookingware. By the
          looks of it, the ghostly chefs seem to use them to prepare food. As
          you peek inside the pots and pans are empty, yet a slight smell of
          food rises from the empty vessels.
        </p>
      </div>
      <p>What will you do?</p>
      <div class="päätösboxi" in:blur={{ duration: 1000 }}>
        <Nappi on:click={hellatoggle}>Take cookware</Nappi>
        <Nappi on:click={halfBackward}>Go back</Nappi>
      </div>

      <!--seuraava vaihe-->
    {:else if step === 5}
      <!--pelaaja saapuu ruokasaliin-->

      <div class="storybox" in:blur={{ duration: 1000 }}>
        <p>
          You arrive at a large hall with high roof. In the middle of the hall
          is several long tables with different tableware, plates and cups. It
          seems you have arrived at the castles dininghall. Behind the tables,
          at the other side of the room there seems to be a door to the next
          area.
        </p>
        <p>
          Suddenly hundreds of ghostly soldiers appear at the tables. They seem
          to be angry and banging the table, chanting something in a language
          you can't understand.
        </p>

        {#if ruokaesilla}
          <h2>*You lost Cauldron*</h2>
          <p>
            The ghostly soldiers are gathering near the cauldron and gorging
            themselves on the phantom food you have just served.
          </p>
        {:else if !ruokaesilla}
          <p>
            As you take a step forwards, all of the hundreds of ghosts turn
            their gaze towards you, as if expecting your next move.
          </p>
        {/if}
      </div>
      <p>What will you do?</p>
      <div class="päätösboxi" in:blur={{ duration: 1000 }}>
        {#if items[1]}
          <Nappi on:click={presentFood}>Present the "food"</Nappi>
        {/if}
        <Nappi on:click={dininghall}>Try to sneak past</Nappi>
        <Nappi on:click={stepBackward}>Go back</Nappi>
      </div>

      <!--puzzle 2 päättyy-->
    {:else if step === 5.5}
      <div class="storybox" in:blur={{ duration: 1000 }}>
        <p>
          You seem to have succesfully snuck past the ghostly soldiers to the
          next room. The door closes and locks behind you.
        </p>
        <p>
          You are standing inside a small room. There is a locked room behind
          you to the prevous room, and a door infront of you to, what you assume
          to be, the next area. There is also a doorway to your left, but it is
          blocked with a heavy iron gate. To the right of you is a wooden
          closet.
        </p>
        <p>
          As you take in your surroundings you start to hear loud footsteps
          coming from your left, behind the iron gate. You listen to your
          instincts and quickly hide inside the wooden closet.
        </p>
        <p>
          Only moments later the iron gate creaks open. You decide to take a
          careful peek at the source of the footsteps. You see a large man with
          a white, human-like mask, wielding a large knife on his right hand and
          breathing heavily. As the gate closes behind the man, he looks at the
          closet you are hiding in for, what feels like eternity. After a while
          the large man goes inside the other door that seems to lead forward.
          After the man has left you step outside of the closet and take a deep
          breath.
        </p>
        <p>It seems you will have company in your next trial...</p>
      </div>
      <div class="päätösboxi" in:blur={{ duration: 1000 }}>
        <Nappi on:click={halfForward}>Go to the next area</Nappi>
      </div>

      <!--puzzle 3 alkaa-->
    {:else if step === 6}
      <!---pelaaja saapuu huoneistojen keskelle-->

      <div class="storybox" in:blur={{ duration: 1000 }}>
        {#if pimeys}
          <p>
            You arrive at a dark corridor as the door behind you closes. You see
            an empty stand for a torch, but it is too dark to see anything else.
          </p>
        {:else if !pimeys}
          <p>Now that the torch lights the corridor, you can see around you.</p>
          <p>
            You stand in a small corridor with 3 small doors leading to three
            different rooms and a large metal door at the end of the corridor
            that seems to be locked.
          </p>
          <p>
            If the legends you have heard are true, the large metal door might
            be the treasure vault of legends. You think you might find a key to
            it from the other rooms.
          </p>
          <p>
            The three different rooms are situated in your north, west and east.
          </p>
          <p>There seems to be no sign of the large man from earlier.</p>
        {/if}
      </div>
      <p>What will you do?</p>
      <div class="päätösboxi" in:blur={{ duration: 1000 }}>
        {#if pimeys}
          <Nappi on:click={pimeystoggle}>Use torch</Nappi>
        {:else if !pimeys}
          <Nappi on:click={enterWest}>Enter the West room</Nappi>
          <Nappi on:click={enterNorth}>Enter the North room</Nappi>
          <Nappi on:click={enterEast}>Enter the East room</Nappi>
          <Nappi on:click={superFullStep}>Go to the metal door</Nappi>
        {/if}
      </div>

      <!--seuraava vaihe-->
    {:else if step === 6.5}
      <!--pelaaja saapuu West huoneeseen-->

      <div class="storybox" in:blur={{ duration: 1000 }}>
        <p>You walk inside to find yourself in a small childs bedroom.</p>
        <p>
          You see a bed, a toy chest, a writing desk and a row of porcelain
          dolls.
        </p>
        {#if wrongplace}
          <p>You find nothing.</p>
        {/if}
      </div>
      <p>What will you do?</p>

      <div class="päätösboxi" in:blur={{ duration: 1000 }}>
        <Nappi on:click={wrongtoggle}>Search the toy chest</Nappi>
        <Nappi on:click={wrongtoggle}>Search the desk</Nappi>
        <Nappi on:click={wrongtoggle}>Search behind the dolls</Nappi>
        <Nappi on:click={piilotoggle}>Hide under the bed</Nappi>
        <Nappi on:click={goBackp3}>Go back to the corridor</Nappi>
      </div>

      <!--seuraava vaihe-->
    {:else if step === 7}
      <!--pelaaja saapuu North huoneeseen-->

      <div class="storybox" in:blur={{ duration: 1000 }}>
        <p>You find yourself in a room that resembles a storageroom</p>
        <p>
          You see a box of potatoes, a wooden closet full of coats, a small
          mousehole in the corner of the room and shelves full of canned goods
        </p>
        {#if wrongplace}
          <p>You find nothing.</p>
        {/if}
        {#if items[0] === 'key'}
          <h2>*Key acquired*</h2>
        {/if}
      </div>
      <p>What will you do?</p>

      <div class="päätösboxi" in:blur={{ duration: 1000 }}>
        <Nappi on:click={wrongtoggle}>Search the box</Nappi>
        <!--pelaaja löytää avaimen-->
        <button
          disabled={$inventory[0] === 'key'}
          on:click={() => inventory.set(['key'])}>Search the mousehole</button
        >
        <Nappi on:click={wrongtoggle}>Search the shelves</Nappi>
        <Nappi on:click={piilotoggle}>Hide inside the closet</Nappi>
        <Nappi on:click={goBackp3}>Go back to the corridor</Nappi>
      </div>

      <!--seuraava vaihe-->
    {:else if step === 7.5}
      <!--pelaaja saapuu East huoneeseen-->

      <div class="storybox" in:blur={{ duration: 1000 }}>
        <p>You enter the room and find yourself standing inside a bathroom.</p>
        <p>
          You see a cupboard full of towels, a sink and a shower with curtains.
        </p>
        {#if wrongplace}
          <p>You find nothing.</p>
        {/if}
      </div>
      <p>What will you do?</p>

      <div class="päätösboxi" in:blur={{ duration: 1000 }}>
        <Nappi on:click={wrongtoggle}>Search the cupboard</Nappi>
        <Nappi on:click={wrongtoggle}>Search under the sink</Nappi>
        <Nappi on:click={piilotoggle}>Hide behind the showecurtain</Nappi>
        <Nappi on:click={goBackp3}>Go back to the corridor</Nappi>
      </div>

      <!--seuraava vaihe-->
    {:else if step === 8}
      <!--pelaaja saapuu metallisen oven eteen-->

      <div class="storybox" in:blur={{ duration: 1000 }}>
        <p>You find yourself infront of a large metal door</p>
        <p>The door has a visible lock on it that is barring your way.</p>
      </div>
      <p>What will you do?</p>
      <div class="päätösboxi" in:blur={{ duration: 1000 }}>
        {#if items[0] === 'key'}
          <Nappi on:click={halfForward}>Use the key</Nappi>
        {/if}
        <Nappi on:click={goBackp3}>Go back to the corridor</Nappi>
      </div>

      <!--puzzle 3 loppuu-->
    {:else if step === 8.5}
      <!--pelaaja saapuu voitto huoneeseen-->

      <div class="storybox" in:blur={{ duration: 1000 }}>
        <p>
          As you turn the key, the heavy metal lock falls on the floor with a
          loud bang.
        </p>
        <p>
          Your adventure is almost finished, will you take the last step inside
          the treasure vault?
        </p>
      </div>
      <div class="päätösboxi" in:blur={{ duration: 1000 }}>
        <Nappi on:click={halfForward}>Victory!</Nappi>
      </div>

      <!--last step!-->
    {:else if step === 9}
      <!--pelaaja löytää täältä ähäkutin-->
      <div class="storybox" in:blur={{ duration: 1000 }}>
        <p>
          As you step inside the vault, you suddenly open your eyes and sit up
          on your bed with cold sweat.
        </p>
        <p>It seems it was all a dream, who knew!</p>
      </div>
      <div class="päätösboxi" in:blur={{ duration: 1000 }}>
        <Nappi on:click={halfForward}>WHAT?!?!</Nappi>
      </div>

      <!--Victory screen!-->
    {:else if step === 9.5}
      <div class="storybox" in:blur={{ duration: 1000 }}>
        <h1>Congratulations {name}!</h1>
        <h2>!YOU WON!</h2>
        <p>And you only died {deaths} while playing, good job!</p>
      </div>
      <div class="päätösboxi" in:blur={{ duration: 1000 }}>
        <Nappi on:click={uudestaan}>I want to play again!</Nappi>
        <Nappi on:click={lopetus}>Quit</Nappi>
      </div>
    {/if}
    <br />

    {#if step > 0}
      <!--reppu näkyy vain jos on päässyt pelaajan nimeämis vaiheesta ohi-->
      <hr class="hrviiva" />
      <br />
      <button class="reppu" on:click={repputoggle}>Show backpack</button>
    {/if}
  </div>
  <div class="container2">
    <footer>
      <p>Made by Toni Virta (AA3098) in 2021.</p>
    </footer>
  </div>
</main>

<style>
  h2 {
    color: rgb(204, 173, 0);
  }

  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
    /*background säädöt*/
    background-image: url('../img/wall.png');
    background-repeat: repeat;
    background-attachment: fixed;
    height: 100vh;
    padding: 0;
  }

  .container {
    max-width: 1300px;
    min-width: 900px;
    min-height: 700px;
    margin: 0 auto;
    /*värit & fontti*/
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    color: rgb(255, 255, 255);
    background-color: rgb(0, 0, 0);
    /*border säädöt*/
    border-radius: 25px;
    border: solid 1em;
    border-image: url('../img/wall2.png') 27 round repeat;
  }

  .container2 {
    max-width: 1300px;
    min-width: 900px;
    max-height: 100px;
    margin: 0 auto;
    /*värit & fontti*/
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    color: rgb(255, 255, 255);
    background-color: rgb(0, 0, 0);
    /*border säädöt*/
    border-radius: 25px;
    border: solid 1em;
    border-image: url('../img/wall2.png') 27 round repeat;
  }

  .storybox {
    max-width: 900px;
    margin: 1em auto;
    padding: 2em;
    border: 2px solid rgb(255, 255, 255);
    border-radius: 25px;
  }

  .reppu {
    background-color: rgb(83, 40, 0);
    color: rgb(255, 255, 255);
    border: 3px solid rgb(34, 16, 0);
    border-radius: 25px;
    padding: 1em;
  }

  .tappaja {
    /*tappajan kuva keskitetään keskelle ruutua*/
    position: fixed;
    width: 500px;
    height: 200px;
    top: 25%;
    left: 50%;
    margin-top: -100px;
    margin-left: -250px;
  }

  .tappajapeek {
    /*tappajan kuva tulee esiin vasemmasta reunasta*/
    position: fixed;
    width: 500px;
    height: 200px;
    top: 25%;
    left: 150px;
    margin-top: -100px;
    margin-left: -250px;
  }

  .hrviiva {
    max-width: 90%;
  }

  footer {
    display: flex;
    justify-content: center;
    align-items: flex-end;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
