<script>
  let formState = {
    hasNewActivity: false,
    newActivityInfos: {
      from: '',
      till: '',
      name: '',
      hours: '',
      found: [],
      healthCareChanged: null,
      healthCareDetails: '',
    },
    extendedActivityTill: '',
  }
  $: activityHasBegun = new Date(formState.newActivityInfos.from) <= new Date()

  function handleChange(event) {
    event.stopPropagation()
    console.log(event, formState)
  }

  function handleSubmit(event) {
    event.preventDefault()
    console.log('submit', formState)
  }
</script>

<style>
  header {
    border-bottom: 1px solid currentColor;
    margin-bottom: 1rem;
  }

  fieldset {
    max-height: 99vh;
    overflow: auto;
    transition: max-height 500ms;
  }
  fieldset:disabled {
    overflow: hidden;
    max-height: 1em;
  }
</style>

<header>
  <h3>Bundesagentur für Arbeit</h3>
  <h1>Veränderungsmittilung</h1>
  <p>SAMPLE DOCUMENT FOR HTML TRAINING</p>
  <p>
    Bitte teilen Sie mit diesem Vordruck Veränderungen in Ihren persöhnlichen
    Verhältnissen, die für die Gewährung Ihrer Leistung maßgeblich sind,
    umgehend mit. Zutreffendes bitte ausfüllen bzw. [x] ankreuzen und an die
    Agentur für Arbeit senden.
  </p>
  <pre hidden>{JSON.stringify(formState, 0, 2)}</pre>
</header>

<form on:submit={handleSubmit} on:change={handleChange}>

  <label>
    <input
      type="checkbox"
      name="hasNewActivity"
      disabled={formState.extendedActivityTill}
      bind:checked={formState.hasNewActivity} />
    Ich nehme eine Tätigkeit auf…
  </label>

  <fieldset name="activity" disabled={!formState.hasNewActivity}>
    <legend for="activity">Angaben zur Tätigkeit</legend>
    <p>
      Ich nehme eine Tätigkeit (Arbeit, selbstständige Tätigkeit, mithelfender
      Familienangehöriger) ab
      <input
        type="date"
        name="activityFrom"
        bind:value={formState.newActivityInfos.from} />
      (bei befristeter Tätigkeit bis
      <input
        type="date"
        name="activityTill"
        bind:value={formState.newActivityInfos.till} />
      ) als (berufliche Tätigkeit)
      <input
        type="text"
        name="activityName"
        bind:value={formState.newActivityInfos.name} />
      auf.
    </p>
    <p>
      Die Tätigkeit umfasst voraussichtlich
      <strong>wöchentlich</strong>
      <label>
        <input
          type="radio"
          name="activityHours"
          bind:group={formState.newActivityInfos.hours}
          value="lt15" />
        weniger als 15 Stunden
      </label>

      <label>
        <input
          type="radio"
          name="activityHours"
          bind:group={formState.newActivityInfos.hours}
          value="ge15" />
        15 Stunden und mehr
      </label>
    </p>
    <p>Wodurch ist die tätigkeit zustande gekommen? (freiwillige Angabe)</p>
    <p>
      <label>
        <input
          type="checkbox"
          name="activityFoundSelf"
          bind:group={formState.newActivityInfos.found}
          value="self" />
        Ich habe mir die tätigkeit selbst gesucht
      </label>
      <label>
        <input
          type="checkbox"
          name="activityFoundAgencyPrivate"
          bind:group={formState.newActivityInfos.found}
          value="agencyPrivate" />
        Die Tätigkeit hat mir ein privater Arbeitsvermittler vermittelt
      </label>
      <label>
        <input
          type="checkbox"
          name="activityFoundAgencyPublic"
          bind:group={formState.newActivityInfos.found}
          value="agencyPublic" />
        Vermittlungsvorschlag der Arbeitsagentur / des Jobcenters
      </label>
    </p>
    <hr />
    {#if activityHasBegun}
      <div class="healthCare">
        <p>
          Zusätzliche Angabe, wenn diese meldung erst
          <strong>nach</strong>
          Arbeitsaufnahme erfolgt:
        </p>
        <p>
          Hat sich die Kassenzugehörigkeit geändert?
          <label>
            <input
              type="radio"
              bind:group={formState.newActivityInfos.healthCareChanged}
              value={true} />
            Ja
          </label>
          {#if formState.newActivityInfos.healthCareChanged}
            <label>
              Neue Krankenkasse (Name, Ort der Geschäftsstelle):
              <input
                type="text"
                bind:value={formState.newActivityInfos.healthCareDetails}
                required />
            </label>
          {/if}
          {#if formState.newActivityInfos.healthCareChanged != true}
            <label>
              <input
                type="radio"
                bind:group={formState.newActivityInfos.healthCareChanged}
                value={false} />
              Nein
            </label>
          {/if}
          {#if formState.newActivityInfos.healthCareChanged != null}
            <label>
              <input
                type="radio"
                bind:group={formState.newActivityInfos.healthCareChanged}
                value={null} />
              (Zurücksetzen)
            </label>
          {/if}
        </p>
        <hr />
      </div>
    {/if}
  </fieldset>

  <label>
    Mein bestehendes Arbeitsverhätnis wird verlängert bis:
    <input
      type="date"
      name="activityExtended"
      disabled={formState.hasNewActivity}
      bind:value={formState.extendedActivityTill} />
  </label>

</form>
