'단순 조회프로그램을 1시간안에 만드는 사람이 있고 10분만에 만드는 사림이 있다.'

단순히 페이지를 만드는 작업에서도 사람마다 걸리는 시간이 많이 다른 사실에 대해서.

화이트 솔루션 페이지를 만들기 위해 필요한 조건은 테이블 작성, 해당 테이블에서 데이터를 조회할 SQL문을 작성한 뒤
화이트 솔루션 시스템관리 메뉴에 따라 오브젝트, 서비스, 메세지, 엔터티, SQL, 공통코드 관리 페이지를 통해 소스 설정과 jsp파일을 매핑하고
메뉴관리에서 해당 페이지의 메뉴를 추가하여 완료한다.
여기에서 관련 코드들을 설정하게 되는데 이 구성은 자신이 구성을 얼마나 제대로 숙지 했고 어떤 흐름으로 동작을 하는지 이해하는 것이 중요하다고 생각한다.
만약 여기서 조회프로그램 작성에 10분이 걸리는 사람은 이 흐름에 대해 자세히 파악하고 있으며 막힘없이 프로그램을 작성할 수 있고
한시간이 걸리는 사람은 이 흐름에 익숙하지않아 속도가 느려진다고 생각한다.

jsp 코드

<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" isELIgnored="false"%>

<%@ taglib prefix="h5" uri="/WEB-INF/h5-ui-core.tld"%>
<%@ taglib prefix="biz" uri="/WEB-INF/h5-biz-core.tld"%>

<h5:H5View>
	<h5:Service>
		<h5:Data>
			<h5:Codes>
				<h5:Code name="grade_cd" target="COMMON_CODE" target="GRADE_CD_MJY">
				</h5:Code>
			</h5:Codes>
			<h5:Messages>
				<h5:Message type="MT_JY0002_01" id="ME_JY0002_01">
				</h5:Message>
				<h5:Message type="MT_JY0002_02" id="ME_JY0002_02">
				</h5:Message>
			</h5:Messages>
		</h5:Data>
		<h5:Actions>
			<h5:Action name="retrieve"  type="SERVICE_CALL" target="JY0002_00_R01">
				<h5:Message id="ME_JY0002_01">
				</h5:Message>
				<h5:ResultEvent>
					<h5:Action type="BIND_DATA">
						<h5:Message id="ME_JY0002_02">
						</h5:Message>
					</h5:Action>
				</h5:ResultEvent>
			</h5:Action>
			<h5:Action name="new" type="NEW_DATA">
				<h5:Message id="ME_JY0002_02">
					<h5:Column id="hr_nm" autoValue="true">
					</h5:Column>
				</h5:Message>
			</h5:Action>
			<h5:Action name="save" type="SERVICE_CALL" target="JY0002_00_S01" serviceCallType="SAVE" useConfirm="true">
				<h5:Message id="ME_JY0002_02">
				</h5:Message>
				<h5:ResultEvent>
					<h5:Action type="ALERT">
					</h5:Action>
					<h5:Action type="RUN_ACTION" target="retrieve">
					</h5:Action>
				</h5:ResultEvent>
				<h5:FaultEvent>
					<h5:Action type="ALERT">
					</h5:Action>
				</h5:FaultEvent>
			</h5:Action>
		</h5:Actions>
	</h5:Service>
	<h5:Layout>
		<h5:SearchBox>
			<h5:Grid id="condition" dataProvider="ME_JY0002_01">
				<h5:GridRow>
					<h5:GridColumn>
						<h5:ComboBox bindingColumn="grade_cd" listProvider="grade_cd" firstItemLabel="전체" firstItemValue="">
						</h5:ComboBox>
					</h5:GridColumn>
					<h5:GridColumn>
						<h5:Label label="이름검색" labelCode="">
						</h5:Label>
					</h5:GridColumn>
				</h5:GridRow>
			</h5:Grid>
			<h5:ActionBox>
				<h5:SearchButton actionName="retrieve" label="조회" labelCode="BTN_RETRIEVE">
				</h5:SearchButton>
			</h5:ActionBox>
		</h5:SearchBox>
		<h5:ContentBox>
			<h5:TitleBox label="간단인사리스트" labelCode="">
				<h5:ActionBox>
					<h5:NewButton actionName="new" label="입력" labelCode="NEW"/>
					<h5:SaveButton actionName="save" label="저장" labelCode="SAVE"/>
				</h5:ActionBox>
			</h5:TitleBox>
			<h5:DataGrid id="mySheetTop" dataProvider="ME_JY0002_02" width="100%" height="30%" showSeq="true" showStatus="true" showDelete="true">
				<h5:DataGridHeaderRow>
					<h5:DataGridHeaderColumn>
						<h5:Label label="사번" labelCode="" />
					</h5:DataGridHeaderColumn>
					<h5:DataGridHeaderColumn>
						<h5:Label label="이름" labelCode="" />
					</h5:DataGridHeaderColumn>
					<h5:DataGridHeaderColumn>
						<h5:Label label="부서" labelCode="" />
					</h5:DataGridHeaderColumn>
					<h5:DataGridHeaderColumn>
						<h5:Label label="직급" labelCode="" />
					</h5:DataGridHeaderColumn>
					<h5:DataGridHeaderColumn>
						<h5:Label label="근무지" labelCode="" />
					</h5:DataGridHeaderColumn>
					<h5:DataGridHeaderColumn>
						<h5:Label label="입사일" labelCode="" />
					</h5:DataGridHeaderColumn>
					<h5:DataGridHeaderColumn>
						<h5:Label label="퇴사일" labelCode="" />
					</h5:DataGridHeaderColumn>
				</h5:DataGridHeaderRow>
				<h5:DataGridBodyRow>
					<h5:DataGridColumn align="center" hidden="false" width="100">
						<h5:TextInput id="hr_no" bindingColumn="hr_no"></h5:TextInput>
					</h5:DataGridColumn>
					<h5:DataGridColumn align="center" hidden="false" width="100">
						<h5:TextInput id="hr_nm" bindingColumn="hr_nm"></h5:TextInput>
					</h5:DataGridColumn>
					<h5:DataGridColumn align="center" hidden="false" width="100">
						<h5:TextInput id="org_nm" bindingColumn="org_nm"></h5:TextInput>
					</h5:DataGridColumn>
					<h5:DataGridColumn align="center" hidden="false" width="100">
						<h5:ComboBox bindingColumn="grade_cd" listProvider="grade_cd" mandatory="true" useFirstItem="true" firstItemLabel="선택" firstItemValue="" />
					</h5:DataGridColumn>
					<h5:DataGridColumn align="center" hidden="false" width="100">
						<h5:TextInput id="place_hometown" bindingColumn="place_hometown"></h5:TextInput>
					</h5:DataGridColumn>
					<h5:DataGridColumn align="center" hidden="false" width="100">
						<h5:TextInput id="hire_ymd" bindingColumn="hire_ymd"></h5:TextInput>
					</h5:DataGridColumn>
					<h5:DataGridColumn align="center" hidden="false" width="100">
						<h5:TextInput id="retire_ymd" bindingColumn="retire_ymd"></h5:TextInput>
					</h5:DataGridColumn>
				</h5:DataGridBodyRow>
			</h5:DataGrid>
		</h5:ContentBox>
	</h5:Layout>
</h5:H5View>

