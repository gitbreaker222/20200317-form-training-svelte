<script>
  const NEW = 'NEW'
  const EXTENDED = 'EXTENDED'

  let formState = {
    activityChangeType: '',
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
    hasAbilityChange: false,
    abilityChangeDetails: {
      fromDate: '',
      hasSicknessInability: false,
      hasSicknessInabilityDoctorCert: false,
      hasSicknessInabilityAccident: false,
      hasExtendedSicknessInability: false,
      isReenabled: false,
    },
    move: false,
    moveDate: '',
    moveConfirmationAttached: false,
    moveDetails: {
      street: '',
      zip: '',
      city: '',
      telephone: '',
    },
    bankChange: false,
    bankIBAN: '',
    bankName: '',
    taxChange: false,
    taxChangeDetails: {
      from: '',
      newClass: '',
      factor: '',
    },
    nameChange: false,
    nameChangeFrom: '',
    nameChangeNew: '',
    pensionChange: false,
    pensionReason: '',
    pensionFrom: '',
    pensionInstitute: '',
    pensionConfirm: false,
    pensionConfirmAt: '',
    continueSearching: false,
    otherChanges: false,
    benefitsReference: false,
    benefitsChildSupport: false,
    benefitsChildSupportId: '',
    benefitsTransition: false,
    benefitsTraining: false,
    accountId: '',
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
    max-height: 200rem;
    overflow: auto;
    transition: max-height 500ms;
  }
  fieldset:disabled {
    overflow: hidden;
    max-height: 1.5rem;
    border: none;
  }

  fieldset legend {
    margin-bottom: 1rem;
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

  <fieldset name="activity">
    <legend for="activity">Angaben zur Tätigkeit</legend>

    <fieldset
      name="newActivity"
      disabled={formState.activityChangeType !== NEW}>
      <legend for="newActivity">
        <label>
          <input
            type="radio"
            bind:group={formState.activityChangeType}
            value={NEW} />
          Ich nehme eine Tätigkeit auf
        </label>
      </legend>

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

    <fieldset
      name="extendedActivity"
      disabled={formState.activityChangeType !== EXTENDED}>
      <legend for="extendedActivity">
        <label>
          <input
            type="radio"
            bind:group={formState.activityChangeType}
            value={EXTENDED} />
          Mein bestehendes Arbeitsverhätnis wird verlängert
        </label>
      </legend>

      <label>
        bis:
        <input
          type="date"
          name="activityExtended"
          disabled={formState.hasNewActivity}
          bind:value={formState.extendedActivityTill} />
      </label>
    </fieldset>

  </fieldset>

  <fieldset name="additional">
    <legend>Weitere Angaben:</legend>

    <fieldset name="workAbility" disabled={!formState.hasAbilityChange}>
      <legend>
        <label>
          <input
            type="checkbox"
            name="hasAbilityChange"
            bind:checked={formState.hasAbilityChange} />
          Arbeitsunfähigkeit:
        </label>
      </legend>

      <label>
        Ich bin ab
        <input type="date" name="abilitChangeFrom" />
        ...
      </label>

      <fieldset disabled={!formState.abilityChangeDetails.hasSicknessInability}>
        <legend>
          <label>
            <input
              type="checkbox"
              name="hasSicknessInability"
              bind:checked={formState.abilityChangeDetails.hasSicknessInability} />
            arbeitsunfähig erkrankt
          </label>
        </legend>

        <label>
          <input
            type="checkbox"
            name="hasSicknessInabilityDoctorCert"
            bind:checked={formState.abilityChangeDetails.hasSicknessInabilityDoctorCert} />
          ärztliche Bescheinigung ist beigefügt
        </label>

        <label>
          <input
            type="checkbox"
            name="hasSicknessInabilityAccident"
            bind:checked={formState.abilityChangeDetails.hasSicknessInabilityAccident} />
          meine Arbeitsunfähigkeit ist durch Unfall verursacht
        </label>
      </fieldset>

      <label>
        <input
          type="checkbox"
          name="hasExtendedSicknessInability"
          bind:checked={formState.abilityChangeDetails.hasExtendedSicknessInability} />
        Weiterhin als arbeitsunfähig erkrankt
      </label>

      <label>
        <input
          type="checkbox"
          name="isReenabled"
          bind:checked={formState.abilityChangeDetails.isReenabled} />
        wieder arbeitsfähig
      </label>
    </fieldset>

    <fieldset disabled={!formState.move}>
      <legend>
        <label>
          <input type="checkbox" name="move" bind:checked={formState.move} />
          Umzug:
        </label>
      </legend>

      <label>
        am:
        <input type="date" name="moveDate" bind:value={formState.moveDate} />
      </label>

      <label>
        <input
          type="checkbox"
          name="moveConfirmationAttached"
          bind:checked={formState.moveConfirmationAttached} />
        Abmelde- und Anmeldebestätigung sind beigefügt
      </label>

      <p>
        Falls für diesen Wohnort eine andere Agentur für Arbeit zuständig ist,
        bitte ich diese für die Zahlung meiner Leistung für zuständig zu
        erklären. Ich werde mich unverzüglich bei der nunmehr zuständigen
        Agentur für Arbeit melden.
      </p>

      <fieldset>
        <legend>Neue Anschrift:</legend>

        <label>
          Straße, Hausnummer:
          <input
            type="text"
            name="street"
            bind:value={formState.moveDetails.street} />
        </label>

        <label>
          Postleitzahl:
          <input
            type="text"
            name="zip"
            bind:value={formState.moveDetails.zip} />
        </label>

        <label>
          Stadt:
          <input
            type="text"
            name="city"
            bind:value={formState.moveDetails.city} />
        </label>

        <label>
          gegebenenfalls neue Telefonnummer:
          <input
            type="telephone"
            name="telephone"
            bind:value={formState.moveDetails.telephone} />
        </label>
      </fieldset>
    </fieldset>

    <fieldset disabled={!formState.bankChange}>
      <legend>
        <label>
          <input
            type="checkbox"
            name="bankChange"
            bind:checked={formState.bankChange} />
          Mein Konto hat sich geändert:
        </label>
      </legend>

      <label>
        IBAN:
        <input type="number" name="bankIBAN" bind:value={formState.bankIBAN} />
      </label>
      <label>
        bei (Kreditinstitut):
        <input type="text" name="bankName" bind:value={formState.bankName} />
      </label>
    </fieldset>

    <fieldset disabled={!formState.taxChange}>
      <legend>
        <label>
          <input
            type="checkbox"
            name="aaaa"
            bind:checked={formState.taxChange} />
          Meine Steuerklasse hat sich geändert:
        </label>
      </legend>

      <label>
        Mit Wirkung ab:
        <input
          type="text"
          name="from"
          bind:value={formState.taxChangeDetails.from} />
      </label>
      <label>
        Neue Steuerklasse:
        <input
          type="text"
          name="newClass"
          bind:value={formState.taxChangeDetails.newClass} />
      </label>
      <label>
        gegebenenfalls Faktor:
        <input
          type="text"
          name="factor"
          bind:value={formState.taxChangeDetails.factor} />
      </label>
    </fieldset>

    <fieldset disabled={!formState.nameChange}>
      <legend>
        <label>
          <input
            type="checkbox"
            name="nameChange"
            bind:checked={formState.nameChange} />
          Mein Name hat sich geändert:
        </label>
      </legend>

      <label>
        Mit Wirkung ab:
        <input
          type="text"
          name="nameChangeFrom"
          bind:value={formState.nameChangeFrom} />
      </label>
      <label>
        Neuer Name:
        <input
          type="text"
          name="nameChangeNew"
          bind:value={formState.nameChangeNew} />
      </label>
    </fieldset>

    <fieldset disabled={!formState.pensionChange}>
      <legend>
        <label>
          <input
            type="checkbox"
            name="pensionChange"
            bind:checked={formState.pensionChange} />
          Ich habe Rente beantragt:
        </label>
      </legend>

      <label>
        wegen:
        <input
          type="text"
          name="pensionReason"
          bind:value={formState.pensionReason} />
      </label>
      <label>
        ab:
        <input
          type="date"
          name="pensionFrom"
          bind:value={formState.pensionFrom} />
      </label>
      <label>
        bei (Rentenversicherungsträger):
        <input
          type="text"
          name="pensionInstitute"
          bind:value={formState.pensionInstitute} />
      </label>
    </fieldset>

    <fieldset disabled={!formState.pensionConfirm}>
      <legend>
        <label>
          <input
            type="checkbox"
            name="pensionConfirm"
            bind:checked={formState.pensionConfirm} />
          Mir wurde Rente bewilligt:
        </label>
      </legend>

      <label>
        Rentenbescheid liegt bei:
        <input
          type="text"
          name="pensionConfirmAt"
          bind:value={formState.pensionConfirmAt} />
      </label>
    </fieldset>

    <fieldset disabled={!formState.continueSearching}>
      <legend>
        <label>
          <input
            type="checkbox"
            name="continueSearching"
            bind:checked={formState.continueSearching} />
          Ich möchte weiter arbeitssuchend geführt werden:
        </label>
      </legend>

      <p>
        Das bedeutet, dass ich weiterhin eine neue Stelle suche, alle Änderungen
        mitteile und Termine bei meiner zuständigen Vermittlungsfachkraft
        wahrnehme.
      </p>
    </fieldset>

    <fieldset disabled={!formState.otherChanges}>
      <legend>
        <label>
          <input
            type="checkbox"
            name="otherChanges"
            bind:checked={formState.otherChanges} />
          Sonstige Änderungen:
        </label>
      </legend>

      <p>
        Vergleiche Nr. 8 des Merkblattes 1 für Arbeitslose (Unterlagen sind
        beigefügt)
      </p>
    </fieldset>

    <fieldset disabled={!formState.benefitsReference}>
      <legend>
        <label>
          <input
            type="checkbox"
            name="benefitsReference"
            bind:checked={formState.benefitsReference} />
          Leistungsbezug:
        </label>
      </legend>

      <p>
        Die Vorstehenden Änderungen beziehen sich auf folgende Leistungen der
        Agentur für Arbeit
      </p>

      <label>
        <input
          type="checkbox"
          name="benefitsChildSupport"
          bind:checked={formState.benefitsChildSupport} />
        Kindergeld:
      </label>
      {#if formState.benefitsChildSupport}
        <label>
          Kindergeldnummer:
          <input
            type="text"
            name="benefitsChildSupportId"
            bind:value={formState.benefitsChildSupportId} />
        </label>
      {/if}

      <label>
        <input
          type="checkbox"
          name="benefitsTransition"
          bind:checked={formState.benefitsTransition} />
        Übergangsgeld:
      </label>
      {#if formState.benefitsTransition}
        <label>
          Kundennummer:
          <input
            type="text"
            name="benefitsTransitionAccountId"
            bind:value={formState.accountId} />
        </label>
      {/if}

      <label>
        <input
          type="checkbox"
          name="accountId"
          bind:checked={formState.benefitsTraining} />
        Berufsausbildungsbeihilfe:
      </label>
      {#if formState.benefitsTraining}
        <label>
          Kundennummer:
          <input
            type="text"
            name="accountId"
            bind:value={formState.accountId} />
        </label>
      {/if}

    </fieldset>

  </fieldset>

</form>
